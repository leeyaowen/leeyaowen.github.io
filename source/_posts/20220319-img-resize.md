---
title: '[python]在必須縮圖至特定長寬條件下維持圖片長寬比並保留最多圖片資訊的方法'
date: 2022-03-19 17:41:16
categories: python
tags: [python, coding, code, opencv, numpy, resize]
---
在圖片要給AI模型時常常需要不同的圖片大小，縮圖即是其中一門學問
這邊以寬640，高480為例
若圖片直接scale到這個大小多有變形問題
尤其直幅圖片影響非常大
這裡提供想法解決這個問題
<!--more-->
如原始圖片為直幅
![](https://photos.smugmug.com/photos/i-gcXwKGT/0/e869363e/X5/i-gcXwKGT-X5.jpg)
經python轉換後維持比例加灰邊至640x480大小
![](https://photos.smugmug.com/photos/i-LLXFGxS/0/3d02a738/O/i-LLXFGxS.jpg)
那以下為轉換的code
{% codeblock python lang:python %}
import cv2
import numpy as np
import os


tf_w = 640
tf_h = 480


def img_resize(img_name):
    img = cv2.imdecode(np.fromfile(img_name, dtype=np.uint8), -1)
    w = img.shape[1]
    h = img.shape[0]
    ratio = h / w
    target_ratio = tf_h / tf_w

    if max(img.shape[:2]) > tf_w:
        interp = cv2.INTER_AREA
    else:
        interp = cv2.INTER_LINEAR

    img_rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)

    if img.shape[:2] == (tf_w, tf_h):
        img_rgb_border = img
    elif ratio == target_ratio:
        img_rgb_border = cv2.resize(img_rgb, (tf_w, int(tf_w * ratio)), interpolation=interp)
    elif (w > h) & (ratio > target_ratio):
        img_rgb = cv2.resize(img_rgb, (int(tf_h * ratio), tf_h), interpolation=interp)
        img_rgb_border = cv2.copyMakeBorder(img_rgb, top=0, bottom=0, left=0, right=tf_w - img_rgb.shape[1],
                                            borderType=cv2.BORDER_CONSTANT,
                                            value=[128, 128, 128])
    elif w > h:
        img_rgb = cv2.resize(img_rgb, (tf_w, int(tf_w * ratio)), interpolation=interp)
        img_rgb_border = cv2.copyMakeBorder(img_rgb, top=0, bottom=tf_h - img_rgb.shape[0], left=0, right=0,
                                            borderType=cv2.BORDER_CONSTANT,
                                            value=[128, 128, 128])
    elif (w <= h) & (ratio > target_ratio) & (ratio <= 1):
        img_rgb = cv2.resize(img_rgb, (int(tf_h * ratio), tf_h), interpolation=interp)
        img_rgb_border = cv2.copyMakeBorder(img_rgb, top=0, bottom=0, left=0, right=tf_w - img_rgb.shape[1],
                                            borderType=cv2.BORDER_CONSTANT,
                                            value=[128, 128, 128])
    elif int(tf_h * ratio) < tf_w:
        img_rgb = cv2.rotate(img_rgb, cv2.ROTATE_90_CLOCKWISE)
        img_rgb = cv2.resize(img_rgb, (int(tf_h * ratio), tf_h), interpolation=interp)
        img_rgb_border = cv2.copyMakeBorder(img_rgb, top=0, bottom=0, left=0,
                                            right=tf_w - img_rgb.shape[1],
                                            borderType=cv2.BORDER_CONSTANT,
                                            value=[128, 128, 128])
    else:
        img_rgb = cv2.rotate(img_rgb, cv2.ROTATE_90_CLOCKWISE)
        img_rgb = cv2.resize(img_rgb, (tf_w, int(tf_w / ratio)), interpolation=interp)
        img_rgb_border = cv2.copyMakeBorder(img_rgb, top=0, bottom=tf_h - img_rgb.shape[0], left=0, right=0,
                                            borderType=cv2.BORDER_CONSTANT,
                                            value=[128, 128, 128])

    os.makedirs('./border', exist_ok=True)
    cv2.imencode('.jpg',
                 cv2.cvtColor(img_rgb_border,
                              cv2.COLOR_RGB2BGR))[1].tofile('./border/%s-border.jpg' % os.path.basename(img_name)[:-4])


if __name__ == '__main__':
    filepath = input('filepath=?\n')
    img_resize(filepath)

{% endcodeblock %}