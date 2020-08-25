---
# Common-Defined params
title: "Ridge와 Lasso, 그리고 베이지안 해석"
date: "2020-04-07T12:00:00+09:00"
description: "Example article description"
categories:
  - "Intro to Statistical Learning"
tags:
  - "Ridge"
  - "Lasso"
menu: side # Optional, add page to a menu. Options: main, side, footer

# Theme-Defined params
thumbnail: "/cover/PRML.jpg" # Thumbnail image
lead: "선형회귀 모델에서 과적화를 막기 위해 인위적으로 도입하는 정규화, 베이지안으로 해석하면 Maximum a Posteriori" # Lead text
comments: false # Enable Disqus comments for specific page
authorbox: true # Enable authorbox for specific page
pager: true # Enable pager navigation (prev/next) for specific page
toc: false # Enable Table of Contents for specific page
mathjax: true # Enable MathJax for specific page
sidebar: "right" # Enable sidebar (on the right side) per page
widgets: # Enable sidebar widgets in given order per page
  - "search"
  - "recent"
  - "taglist"

---

![D847DC6E01](/posts/ISL/PRMLCH02/E8EFB1D413.jpeg)

![D847DC6E01](/posts/ISL/PRMLCH02/E8EFB1D414.jpeg)

![D847DC6E01](/posts/ISL/PRMLCH02/E8EFB1D415.jpeg)

![D847DC6E01](/posts/ISL/PRMLCH02/E8EFB1D416.jpeg)

![D847DC6E01](/posts/ISL/PRMLCH02/E8EFB1D417.jpeg)

![D847DC6E01](/posts/ISL/PRMLCH02/E8EFB1D418.jpeg)

![D847DC6E01](/posts/ISL/PRMLCH02/E8EFB1D419.jpeg)

![D847DC6E01](/posts/ISL/PRMLCH02/E8EFB1D420.jpeg)

![D847DC6E01](/posts/ISL/PRMLCH02/E8EFB1D421.jpeg)

![D847DC6E01](/posts/ISL/PRMLCH02/E8EFB1D422.jpeg)

![D847DC6E01](/posts/ISL/PRMLCH02/E8EFB1D423.jpeg)

![D847DC6E01](/posts/ISL/PRMLCH02/E8EFB1D424.jpeg)

![D847DC6E01](/posts/ISL/PRMLCH02/E8EFB1D425.jpeg)

![D847DC6E01](/posts/ISL/PRMLCH02/E8EFB1D426.jpeg)

![D847DC6E01](/posts/ISL/PRMLCH02/E8EFB1D427.jpeg)

![D847DC6E01](/posts/ISL/PRMLCH02/E8EFB1D428.jpeg)

![D847DC6E01](/posts/ISL/PRMLCH02/E8EFB1D429.jpeg)

#### References

1. Pattern Recognition and Machine Learning, Bishop, 2006
2. Introduction to Statistical Learning, James et al, 2013