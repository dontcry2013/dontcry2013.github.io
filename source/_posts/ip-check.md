---
title: IP审查
date: 2016-06-21 13:35:21
categories: [Security]
tags: [Java]
---
公共服务难免有诸多接口暴露给外部调用，其中不乏有恶意请求，再没有针对性的业务处理之前，ip审查是行之有效的防御办法。
<!--more-->

在servlet中的调用
``` java
	private static IpCounter ipSmsCounter;
	static boolean checkSmsIP(HttpServletRequest req){
		String sIp = IPUtils.getIpAddr(req);
		Long lIp = IPUtils.ipToLong(sIp);
		Boolean isInBlackList = BlackIpFileHelper.getInst().checkIp(lIp);
		if (isInBlackList) {
			return false;
		}

		Boolean isInWhiteList = WhiteIpFileHelper.getInst().checkIp(lIp);
		if (isInWhiteList) {
			// 白名单中，跳过计数
			return true;
		} else {
			Boolean isInTempBlackList = TempBlackIpFileHelper.getInst().checkIp(lIp);
			if (isInTempBlackList) {
				return false;
			}
			if(ipSmsCounter==null){
				ipSmsCounter = new IpCounter(8, 24*60*60*1000L); //一天8次
			}
			ipSmsCounter.add(lIp);
		}
		return true;
	}

```

``` java
package com.xxx.ippermit;

import java.util.Date;
import java.util.Map.Entry;
import java.util.concurrent.ConcurrentHashMap;

public class IpCounter {

	private long lastCheckTime = 0L;
	private int maxCount = 7;
	private Long interval = 30000L;

	public IpCounter(int maxCount, Long interval) {
		this.maxCount = maxCount;
		this.interval = interval;
	}

	// ip计数池
	private ConcurrentHashMap<Long, Integer> ipCounter = new ConcurrentHashMap<Long, Integer>();

	public boolean add(Long lIp) {
		if (ipCounter.containsKey(lIp)) {
			int times = ipCounter.get(lIp) + 1;
			if (times > maxCount) {
				TempBlackIpFileHelper.getInst().addIp(lIp, new Date().getTime() + 24 * 3600 * 1000);
				return false;
			}
			ipCounter.put(lIp, times);
		} else {
			ipCounter.put(lIp, 1);
		}

		// 指定间隔时间后，将计数器中超过限制的加入临时黑名单
		Long curTime = (new Date()).getTime();
		if (lastCheckTime == 0) {
			lastCheckTime = curTime;
		}
		if ((curTime - lastCheckTime) > interval) {
			synchronized (this) {
				for (Entry<Long, Integer> e : ipCounter.entrySet()) {
					if (e.getValue() > maxCount) {
						TempBlackIpFileHelper.getInst().addIp(lIp, new Date().getTime() + 24 * 3600 * 1000);
					}
				}
				lastCheckTime = (new Date()).getTime();
				ipCounter.clear();
			}
		}
		return true;
	}
}

```
``` java
package com.xxx.ippermit;

import java.io.BufferedReader;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.InetAddress;
import java.net.UnknownHostException;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Map.Entry;
import java.util.concurrent.ConcurrentHashMap;

import org.apache.commons.lang.StringUtils;

import com.xxx.utils.FileHelper;
import com.xxx.utils.IPUtils;

public class TempBlackIpFileHelper {
	private static TempBlackIpFileHelper inst = new TempBlackIpFileHelper(FileHelper.combinPath(Configuration.getStorePath(), "ippermit/tempblackip.txt"));

	public static TempBlackIpFileHelper getInst() {
		return inst;
	}

	private ConcurrentHashMap<Long, Long> ipMap = new ConcurrentHashMap<Long, Long>();
	private String fileName = "";
	private Long codeFileDate = 0L;
	private File file = null;
	private Long lastCheckTime = 0L;
	private Long lastWriteTime = 0L;
	private SimpleDateFormat formatter = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");


	private TempBlackIpFileHelper(String sFileName) {
		fileName = sFileName;
		file = new File(fileName);
	}

	private synchronized void checkReload() {
		Long curTime = (new Date()).getTime();

		if (lastWriteTime == 0) {
			lastWriteTime = curTime;
		}

		// 重写文件
		if ((curTime - lastWriteTime) > 600000) {
			synchronized (inst) {
				// 10秒检查一次
				lastWriteTime = curTime;

				// 重新输出文件
				file.delete();
				FileWriter fWriter = null;
				PrintWriter pWriter = null;
				try {
					fWriter = new FileWriter(file, true);
					pWriter = new PrintWriter(fWriter);
					for (Entry<Long, Long> e : ipMap.entrySet()) {
						if (e.getValue() > curTime) {
							pWriter.println(IPUtils.longToIp(e.getKey()) + "|" + formatter.format(new Date(e.getValue())));
						}
					}
				} catch (IOException e) {
					e.printStackTrace();
				} finally {
					try {
						if (pWriter != null) {
							pWriter.close();
						}

						if (fWriter != null) {
							fWriter.close();
						}
					} catch (IOException e) {
					}
				}
				codeFileDate = file.lastModified();
			}
		}

		// 重加载文件
		if ((curTime - lastCheckTime) > 10000) {
			// 10秒检查一次
			lastCheckTime = curTime;

			if (file.exists()) {
				if (file.lastModified() != codeFileDate) {
					ipMap = loadMap(null);
				}
			}
		}

	}

	private ConcurrentHashMap<Long, Long> loadMap(ConcurrentHashMap<Long, Long> map) {
		if (map == null) {
			map = new ConcurrentHashMap<Long, Long>();
		}
		BufferedReader reader = null;
		try {
			FileInputStream fis = new FileInputStream(file);
			InputStreamReader isr = new InputStreamReader(fis, "UTF-8");
			reader = new BufferedReader(isr);
			String tempString = null;
			while ((tempString = reader.readLine()) != null) {
				if (StringUtils.isNotEmpty(tempString)) {
					String[] arr = tempString.split("\\|");
					try {
						map.put(IPUtils.ipToLong(arr[0]), formatter.parse(arr[1].trim()).getTime());
					} catch (Exception e) {
						System.out.println("动态ip黑名单加载失败");
					}
				}
			}
			reader.close();
			codeFileDate = file.lastModified();
			return map;
		} catch (IOException e) {
			System.out.println("动态ip黑名单加载失败");
			return map;
		} finally {
			if (reader != null) {
				try {
					reader.close();
				} catch (IOException e1) {
				}
			}
		}
	}

	public synchronized void append(String data) {
		FileWriter fWriter = null;
		PrintWriter pWriter = null;
		try {
			fWriter = new FileWriter(file, true);
			pWriter = new PrintWriter(fWriter);
			pWriter.println(data);
		} catch (IOException e) {
			e.printStackTrace();
		} finally {
			try {
				if (pWriter != null) {
					pWriter.close();
				}

				if (fWriter != null) {
					fWriter.close();
				}
			} catch (IOException e) {
			}
		}
	}

	public boolean checkIp(Long lIp) {
		checkReload();
		if (ipMap.containsKey(lIp)) {
			return ipMap.get(lIp) > new Date().getTime();
		} else {
			return false;
		}
	}
	/**
	 * 百度蜘蛛反IP地址查询
	 *
	 * @param inetAddress
	 * @param lIp
	 * @return
	 */
	public boolean checkBaiduSpider(Long lIp) {
		boolean flag = false;
		InetAddress inetAddress;
		try {
			String ip = IPUtils.longToIp(lIp);
			inetAddress = InetAddress.getByName(ip);
			if (inetAddress != null) {
				String hostName = inetAddress.getHostName();
				if (StringUtils.isNotEmpty(hostName)) {
					if (hostName.contains("baidu.com") || hostName.contains("baidu.jp")) {
						flag = true;
						System.out.println(ip + "|" + inetAddress.getHostName() + "|" + inetAddress.getHostAddress() + "|" + inetAddress.getAddress());
					}
				}
			}
		} catch (Exception e) {
			return false;
		}
		return flag;
	}

	
	public void addIp(Long lIp, Long time) {
		if (checkBaiduSpider(lIp) == false) {
			synchronized (inst) {
				ipMap.put(lIp, time);
				append(IPUtils.longToIp(lIp) + "|" + formatter.format(new Date(time)));
			}
		}
	}
}

```

Configuration读取文件路径
``` java
package com.xxx.ippermit;

import java.io.IOException;
import java.util.Properties;

public class Configuration {
	private static Properties properties;

	static {
		try {
			properties = new Properties();
			properties.load(Configuration.class.getResourceAsStream("/xxx.properties"));
		} catch (IOException e) {
			System.out.print("读取xxx.properties配置文件出错");
		}
	}

	public static String getObject(String key) {
		return properties.getProperty(key);
	}

	public static String getStorePath() {
		return getObject("storePath");
	}

}
```

一些工具代码
``` java
package com.xxx.utils;

public class FileHelper {
	public static String combinPath(String path1, String path2) {
		path1 = path1.trim().replace("\\", "/");
		path2 = path2.trim().replace("\\", "/");

		if (path1.endsWith("/")) {
			if (path2.startsWith("/")) {
				return path1.substring(0, path1.length() - 1) + path2;
			} else {
				return path1 + path2;
			}
		} else {
			if (path2.startsWith("/")) {
				return path1 + path2;
			} else {
				return path1 + "/" + path2;
			}
		}
	}
}
```

``` java
package com.xxx.utils;

import javax.servlet.http.HttpServletRequest;

public class IPUtils {
	/**
	 * @info:通过request 获得用户的真实的Ip
	 * @param request
	 * @return String
	 */
	public static String getIpAddr(HttpServletRequest request) {
		String ip = request.getHeader("x-forwarded-for");
		if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
			ip = request.getHeader("Proxy-Client-IP");
		}
		if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
			ip = request.getHeader("WL-Proxy-Client-IP");
		}
		if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
			ip = request.getRemoteAddr();
		}

		// String ip = request.getRemoteAddr();

		if (ip != null && ip.length() != 0) {
			if (ip.indexOf(",") >= 0) {
				String[] array = ip.split(",");
				for (int i = 0; i < array.length; i++) {
					if (array[i] != null
							&& !("unknown".equalsIgnoreCase(array[i]))) {
						ip = array[i];
						break;
					}
				}
			}
		}
		if (ip == null)
			ip = "";
		return ip.trim();
	}

	public static Long ipToLong(String ip) {
		String[] arr = ip.split("\\.");
		return (Long.parseLong(arr[0]) << 24) //
				+ (Long.parseLong(arr[1]) << 16) //
				+ (Long.parseLong(arr[2]) << 8) //
				+ (Long.parseLong(arr[3]));
	}

	public static String longToIp(long ipLong) {
		String strip = "";
		Long ip1, ip2, ip3, ip4;
		ip1 = (ipLong >> 24) & 255;
		ip2 = (ipLong >> 16) & 255;
		ip3 = (ipLong >> 8) & 255;
		ip4 = ipLong & 255;
		strip = ip1.toString() + "." + ip2.toString() + "." + ip3.toString()
				+ "." + ip4.toString();
		return strip;
	}

}
```