# APK-Analyzer
APK分析練習

```
2018/06/04
針對提供的APK進行反組譯，使用工具：apktool、dex2jar、jd-gui。
===========================================================
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

2.利用Genymotion建立測試環境，觀看程式執行過程
```

```
2018/06/05
延續昨天的進度，進行程式碼分析
===================================
利用dex2jar對classes.dex進行重組譯得到jar檔，使用jd-gui觀看原始程式碼。
  =>從MainActivity入手，發現程式有隱藏圖示與取得管理員權限的動作，並啟動Service。
  =>程式設定每40秒呼叫TaskRequest
```

```
2018/06/06
延續昨天的進度，持續進行程式碼分析
===================================
分析TaskRequest的功能
  =>此Class在剛執行時會抓取被害人手機號碼、MODEL、SDK_VERSION並將其傳送至C2伺服器。
  =>接著依據不同的參數設定執行相對應的行為
    ->100：傳送手機號碼至SERVER端
    ->0：發送詐騙簡訊
    ->2：收集聯絡人資料
    ->3：持續在背景監聽
```

```
2018/06/07
找出APK中的各項URL功用及觸發條件
=====================================
Forward - 攔截簡訊
ForwardContacts - 竊取通訊錄
Log - 發送紀錄
NEWURL - 新增連結要求
Request(phone、model+sdk_version) - 傳送手機資訊(號碼、MODEL、SDK_VERSION)給C2 Server
Send(phone) - 傳送被害人手機號碼給C2 Server
SendContacts - 傳送手機聯絡人給C2 Server
SendToContacts- 傳送詐騙簡訊給手機聯絡人
```

```
2018/06/08
繪製apk流程圖
=====================================
將APK的整體行為進行整理，以流程圖表示。
```

```
2018/06/11
與組員討論流程圖、確認各行為間的關聯性
=====================================
重新檢視先前的行為判斷是否有疏漏的地方
```

```
2018/06/12
追蹤smali code，檢查是否遺漏惡意行為
=====================================
由於先前已追蹤JAVA Code為主，但部分程式碼流程有疑慮，因此開始追蹤Smali Code進行佐證。
```

```
2018/06/13
持續追蹤smali code
=====================================
發現在TaskRequest中，dex2jar反組譯的JAVA Code與Smali Code不符。
  =>針對此處進行分析
```

```
2018/06/14
持續追蹤smali code
=====================================
延續前日的結果，對於該class參數的設定分析
  =>發現在smali code中，另有一參數2會觸發行為。
```

```
2018/06/15
繪製apk流程圖
=====================================
將此週的發現進行整理，更新流程圖表示。
```

```
2018/06/19
持續追蹤smali code
=====================================
```

```
2018/06/20
持續追蹤smali code
=====================================
```

```
2018/06/22
訂定分析APK流程SOP
=====================================
訂定初稿，缺失如下：
1.動態分析流程不清楚，未定義明確。
2.靜態分析考慮狀況不夠周全
3.未加入自動化引擎
```

```
2018/06/25
編寫Bash script自動化分析apk
=====================================
研究腳本怎麼寫...
mkdir reports     //產生報告存放的資料夾

for apk in $(find $1 -maxdepth 1 -name "*.apk")   //從存放apk的資料夾中一一讀入apk
do
        /home/yrkuo/gu/details/dt/bin/python joe_security.py $apk > $apk.txt    //將每一隻apk產出的報告存成txt檔
done
```

```
2018/06/26
編寫Bash script自動化分析apk
=====================================
修改腳本
ls -d $1reports || mkdir $1reports     //先檢查是否已存在資料夾，沒有則產生報告存放的資料夾（位置為apk資料夾下）

for apk in $(find $1 -maxdepth 1 -name "*.apk")
do
        //將原先只能存放在apk資料夾中的報告改為可以指定存放資料夾，並以apk名稱命名報告txt檔
        //${apk##*/}這寫法可以將原先含路徑的參數改為只保留最後一個/左邊的文字
        /home/yrkuo/gu/details/dt/bin/python joe_security.py $apk > $1reports/${apk##*/}.txt  
done

```

```
2018/06/27
編寫Bash script自動化分析apk
=====================================
修改腳本
IFS=$(echo -en "\n\b")    //加入此行的目的是解決若apk檔名有空格的情況下會發生重複讀檔的BUG

ls -d $1reports || mkdir $1reports     //先檢查是否已存在資料夾，沒有則產生報告存放的資料夾（位置為apk資料夾下）

for apk in $(find $1 -maxdepth 1 -name "*.apk")
do
        //將原先只能存放在apk資料夾中的報告改為可以指定存放資料夾，並以apk名稱命名報告txt檔
        //${apk##*/}這寫法可以將原先含路徑的參數改為只保留最後一個/左邊的文字
        /home/yrkuo/gu/details/dt/bin/python joe_security.py $apk > $1reports/${apk##*/}.txt  
done
-------------------------------------
echo -n 不換行輸出
echo -e 處理特殊字元
-------------------------------------
```

```
2018/06/29
討論修改apk分析流程SOP
=====================================
加入自動化引擎分析，抓出APK可疑行為的切入點，並將靜態判斷加密/加殼/混淆的流程
```
