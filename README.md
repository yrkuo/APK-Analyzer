# APK-Analyzer
APK分析練習

```
2018/06/04
針對提供的APK進行反組譯，使用工具：apktool、dex2jar、jd-gui。
1.查看AndroidManifest.xml，找尋切入點
  收集使用的權限:
    android.permission.READ_PHONE_STATE
    android.permission.SEND_SMS
    android.permission.READ_SMS
    android.permission.WRITE_SMS
    android.permission.RECEIVE_SMS
    android.permission.INTERNET
    android.permission.READ_CONTACTS
    android.permission.RECEIVE_BOOT_COMPLETED
  收集APK使用的Activity、Service、Receiver。
  
================================================

2.利用Genymotion建立測試環境，觀看程式執行過程
```

```
2018/06/05
延續昨天的進度，進行程式碼分析
1.利用Genymotion建立測試環境，觀看程式執行過程
2.比對程式碼找出不合理的地方
```

```
2018/06/06
今天嘗試建立分析環境
1.利用VirtualBOX安裝Santoku linux
2.嘗試使用androguard提供的各種工具分析APK
```

```
2018/06/07
找出APK中的各項URL功用及觸發條件
```

```
2018/06/08
繪製apk流程圖
```

```
2018/06/11
與銘峯討論流程圖、確認各行為間的關聯性。
```

```
2018/06/12
追蹤smali code，檢查是否遺漏惡意行為。
```

```
2018/06/13
持續追蹤smali code
*發現dex2jar中的程式碼與smali不符
```
