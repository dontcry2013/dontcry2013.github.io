---
title: 图像识别之灰度，二值化，高斯模糊处理
date: 2016-04-26 11:43:24
categories: OCR
tags: [C]
---
在图像处理中，灰度和二值化是其他操作的前提。灰度的方式有多种，比较简单的处理就是取RGB平均值，从而使RGBA8888格式的图片转化为了单色道的灰度图，每个像素存储在一个char中而非四个char。
高斯模糊，图像处理软件会提供"模糊"（blur）滤镜，使图片产生模糊的效果。"模糊"的算法有很多种，其中有一种叫做"高斯模糊"（Gaussian Blur）。它将正态分布（又名"高斯分布"）用于图像处理。
二值化，设定某个条件将像素转为黑白两种色，这里设置的条件为：是否大于色素平均值再减去40，即buffer_temp[x]>avage_char-40?255:0。
<!-- more -->

```JavaScript
typedef struct {
    unsigned char * buffer;
    int width;
    int height;
    int charcount;
} IMAGE;

//gauss start
#define PI 3.14159265

typedef enum {
    FILTER_AVG,
    FILTER_GAUSS
} filter_type;

typedef struct {
    int radius;
    double **matrix;
    int type;
} FILTER;

double gauss_2d(int x, int y, double sigma) {
    double result = 1.0 / (2 * PI * sigma * sigma);
    result *= exp(-(x*x+y*y)/(2 * sigma * sigma));
    return result;
}

FILTER *filter_create_gauss(int radius, double sigma) {
    //Allocate mem for the structure
    FILTER *filter = (FILTER*) malloc(sizeof(FILTER));
    filter->radius = radius;
    filter->type = FILTER_GAUSS;

    //Used for iterations
    int i, j;

    //The matrix width and height
    int dim = 2*radius+1;

    //Alocate mem for the matrix
    filter->matrix = (double**) malloc(dim * sizeof(double*));
    for(i = 0; i < dim; i++)
        filter->matrix[i] = (double*) malloc(dim * sizeof(double));

    //Calculate
    double sum = 0.0;
    for(i = -radius; i <= radius; i++)
        for(j = -radius; j <= radius; j++) {
            filter->matrix[i+radius][j+radius] = gauss_2d(j, i, sigma);
            sum += filter->matrix[i+radius][j+radius];
        }

    //Correct so that the sum of all elements ~= 1
    for(i = 0; i < 2*radius+1; i++)
        for(j = 0; j < 2*radius+1; j++)
            filter->matrix[i][j] /= sum;

    return filter;
}

void filter_free(FILTER *filter) {
    //Free matrix
    int dim=2*filter->radius+1, i;
    for(i = 0; i < dim; i++)
        free(filter->matrix[i]);
    free(filter->matrix);

    //Free filter
    free(filter);
}

void apply_to_pixel(int x, int y,unsigned char* original,unsigned char *result, FILTER *filter,int width,int height) {

    if(x<filter->radius || y<filter->radius || x>=width-filter->radius || y>=height-filter->radius) {
        result[width*y+x] = original[width*y+x];
        return;
    }

    int i, j;
    unsigned char res = 0;
    double fil;

    for(i = -filter->radius; i <= filter->radius; i++)
        for(j = -filter->radius; j <= filter->radius; j++) {
            fil = filter->matrix[i+filter->radius][j+filter->radius];
            res += fil * original[ (y+i)*width+x+j];
        }

    result[width*y+x] = res;
}

unsigned char * apply_gauss_filter(unsigned char* original,int w,int h,FILTER* filter){
    unsigned char * result = (unsigned char*)malloc(w*h);
    int x, y;
    for(y = 0; y < h ; y++)
        for(x = 0; x < w; x++)
            apply_to_pixel(x, y, original, result, filter,w,h);
    return result;
}

static FILTER* filter = NULL;

unsigned char* gauss(unsigned char* image, int width ,int height){
    if(filter==NULL){
        filter = filter_create_gauss(2, 1);
    }
    unsigned char* result = apply_gauss_filter(image,width,height,filter);
    return result;
}
//gauss end

IMAGE prepare(unsigned char* buffer,int width,int height) {
    IMAGE image;

    unsigned char* buffer_temp;
    int length = width*height;
    int x;

    //灰度
    buffer_temp = (unsigned char*)malloc(length);
    unsigned char* p;
    double avage = 0;
    for(x = 0 ; x < length ; x ++){
        p = buffer+x*4;
        float v = (p[0] + p[1] + p[2])/3.0f;
        avage = avage + v;
        buffer_temp[x] = (unsigned char)v;
    }
    avage = avage/length;

    //高斯模糊
    unsigned char* result = gauss(buffer_temp, width ,height);
    free((void*)buffer_temp);
    buffer_temp = result;

    //二值化
    unsigned char avage_char = (unsigned char)avage;
    for(x = 0 ; x < length ; x ++){
        buffer_temp[x] = buffer_temp[x]>avage_char-40?255:0;
    }

    image.buffer = buffer_temp;
    image.width = width;
    image.height = height;
    return image;
}
```
参考：
[高斯模糊的算法](http://www.ruanyifeng.com/blog/2012/11/gaussian_blur.html)
[高斯模糊算法的实现和优化](http://blog.csdn.net/markl22222/article/details/10313565)