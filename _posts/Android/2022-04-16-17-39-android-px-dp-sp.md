---
layoout: post
title:  "dp, sp, px"
crawlertitle: post_android
date:   2022-04-16 17:39:00 +0900
categories: android
summary: "Android에서 사용되는 단위 px, dp, sp에 대한 설명"
--- 
##### 1. px(pixel; 픽셀)  
- 화면을 구성하는 최소 단위  
- 해상도 관계없이 같은 크기를 갖습니다.  
- px로 Layout을 설정하면 여러 단말기의 화면에서 제각각 다르게 보여질 수 있으므로 사용을 권장하지 않습니다.  

##### 2. dp(density-independent pixels; 밀도 독립형 픽셀)  
- dp, dip  
- 밀도가 서로 다른 화면에서 UI 표시 크기를 유지하려면 dp를 측정 단위로 사용해야 합니다.  
- 1dp ≒ 1px : 1dp는 160dpi 기준 밀도(medium-density screen)의 1픽셀과 거의 동일한 가상 픽셀 단위  
- Android에서는 밀도마다 적합한 실제 픽셀 수로 변환합니다.  
- 여러 단말기의 화면에도 거의 비슷한 형태로 출력됩니다.  
- View나 2개의 View 사이의 간격을 지정할 때 사용합니다.  

##### 3. sp(scalable pixels; 확장 가능 픽셀)  
- sp, sip  
- dp와 같은 크기를 사용합니다.  
- 사용자가 설정한 텍스트 크기에 따라 조절됩니다.  
- Layout 크기에는 사용하지 않습니다.  
- 텍스트 크기를 지정할 때 사용합니다.  

###### [참고]  
- [다양한 화면 크기 지원](https://developer.android.com/training/multiscreen/screendensities)  