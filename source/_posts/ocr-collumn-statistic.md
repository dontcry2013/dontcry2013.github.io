---
title: 图像识别之像素列统计
date: 2016-04-19 15:16:18
categories: OCR
tags: [C]
---

prerequisite：经过二值化，每个像素点只有0（黑色）和255（白色）两个值。
<!-- more -->

``` c
 //行统计
    float rowTotal = 0;
    float rowMax = 0;
    float maxRowIndex = 0;
    float rowCount[height];

    for(y = 0 ;y <height;y++){
        rowCount[y] = 0;
        for( x = 0;x<width;x++){
        	// 按行累加，黑为1，白为0
            rowCount[y] = rowCount[y] + (1.0-(buffer[y*width+x]/255.0f));
        }
        // 化一后的像素值总和
        rowTotal = rowTotal + rowCount[y];
        // 统计最大值和位置
        if(rowCount[y]>rowMax){
            rowMax = rowCount[y];
            maxRowIndex = y;
        }
    }
    
    float rowAverage = rowTotal/height;
    
    
    int y_start = maxRowIndex;
    int y_end = maxRowIndex;
    
    // 向上寻找波形的边界
    for(;y_start>0;){
        if(rowCount[y_start]> rowAverage/3){
            y_start--;
        } else {
            break;
        }
    }
    
    // 向下寻找波形的边界
    for(;y_end<height;){
        if(rowCount[y_end] > rowAverage/3){
            y_end++;
        } else {
            break;
        }
    }
    
	// 分配边长为height的正方形内存空间
    buffer_temp = (unsigned char*)malloc(height*height);
    memset(buffer_temp, 255, height*height);
    int i;

    // 一维数组转化为二维图片
    for (y = 0; y < height; y++) {
    	// 设最大值的图像为全黑，从0点开始画白点，得到图像的高为0, 其余按比例设置
    	// index为需要画白点的长度
        int index = (1.0 - rowCount[y] / rowMax) * height;
        if (y == height - 1) {
            index = (1.0 - rowAverage / rowMax) * height;
        }
        // 边界为黑
        if (y == y_start || y == y_end) {
            index = 0;
        }
        // 水平方向，反向画白点
        for (i = 0; i < height; i++) {
            if (i > index) {
                buffer_temp[y * height + i] = 0;
            }
        }
    }

    image.buffer = buffer_temp;
    image.width = height;
    image.height = height;
    return image;
     
```


``` c
  //列统计(y_start和y_end的范围内进行)
    float colTotal = 0;
    float colMax = 0;
    float colColIndex = 0;
    float colCount[width];

    for ( x = 0; x < width; x++) {
        colCount[x] = 0;
        for ( y = y_start; y <= y_end; y++) {
        	// 按列累加，黑为1，白为0
            colCount[x] = colCount[x] + (1.0 - (buffer[y * width + x] / 255.0f));
        }
        // 化一后的像素值总和
        colTotal = colTotal + colCount[x];
        // 统计最大值和位置
        if (colCount[x] > colMax) {
            colMax = colCount[x];
            colColIndex = x;
        }
    }

    float colAverage = colTotal / width;

    // 分配边长为width的正方形内存空间
    buffer_temp = (unsigned char *) malloc(width * width);
    memset(buffer_temp, 255, width * width);

    // 一维数组转化为二维图片
    for ( x = 0; x < width; x++) {
    	// 设最大值为全黑，从0点画白点的高为0, 其余按比例设置
    	// index为需要画白点的高度
        int index = (1.0 - colCount[x] / colMax) * width;
        if (x == width - 1) {
            index = (1.0 - colAverage / colMax) * width;
        }

        for ( y = 0; y < width; y++) {
            if (y > index) {
            	// 竖直方向，反向画白点
                buffer_temp[y * width + x] = 0;
            }
        }
    }
    image.buffer = buffer_temp;
    image.width = width;
    image.height = width;
    return image;

```