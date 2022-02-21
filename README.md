# Modbus 資料傳到 IoT-Hub 再存入 PostgreSQL

* [測試環境](#測試環境)
* [Install](#Install)
* [範例](#範例)

<h1 id="測試環境"> 測試環境 </h1>

* Raspberry Pi 4 Model B 4GB

<h1 id="Install"> Install </h1>

>檢查有沒有裝 Node.js

```
node -v
```

>沒有的話，執行以下指令

```
sudo apt-get install nodejs-legacy
```

>檢查有沒有裝 npm

```
npm -v
```

>沒有的話，執行以下指令

```
sudo apt-get install npm
```

>安裝 Node-RED

```
sudo npm install -g --unsafe-perm node-red node-red-admin 
```

>啟動 Node-RED service

```
node-red-start
```

>啟動後，打開瀏覽器輸入樹梅派 IP，Port 1880 就可以進入

```
http://xxx.xxx.xxx.xxx:1880
```

>停止 Node-RED service

```
node-red-stop
```

>假設部屬完後 service 掛掉，可以進入安全模式把有衝突或打錯的東西改掉再進行一次部屬，就可以再用上面正常方式進入

```
node-red --safe
```

>設定開機自動啟動 Node-RED service

```
sudo systemctl enable nodered.service
```

<h1 id="範例"> 範例 </h1>

>安裝 Modbus、IoT Hub 的套件

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221008-2.PNG)

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221009-2.PNG)

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221010-2.PNG)

>直接匯入以下 Json

```Json
[{"id":"d6f7e07434fae5c3","type":"tab","label":"Flow2","disabled":false,"info":"","env":[]},{"id":"f775e252.a49f2","type":"debug","z":"d6f7e07434fae5c3","name":"Log","active":true,"console":"false","complete":"true","x":1170,"y":280,"wires":[]},{"id":"817f33a3.ddf5f","type":"azureiothubreceiver","z":"d6f7e07434fae5c3","name":"Azure IoT Hub Receiver","credentials":{},"x":880,"y":340,"wires":[["c2825fc8.d6323"]]},{"id":"c2825fc8.d6323","type":"debug","z":"d6f7e07434fae5c3","name":"Log","active":true,"console":"false","complete":"true","x":1170,"y":340,"wires":[]},{"id":"d934aff363c2ead2","type":"azureiothub","z":"d6f7e07434fae5c3","name":"Azure IoT Hub","protocol":"mqtt","credentials":{},"x":900,"y":280,"wires":[["f775e252.a49f2"]]},{"id":"7527b5fdb64f6132","type":"function","z":"d6f7e07434fae5c3","name":"","func":"msg.payload = {\n\tdeviceId: \"test777\",\n    key: \"aUecqAb3OJ2CNak4MU5X9pGES2QCumQs2L0XEb2DNPM=\",\n    protocol: \"mqtt\",\n    data: {\n        a: msg.payload[0],\n        b: msg.payload[1],\n        c: msg.payload[2],\n        d: msg.payload[3],\n        e: msg.payload[4],\n        f: msg.payload[5],\n        g: msg.payload[6],\n        h: msg.payload[7],\n        i: msg.payload[8],\n        j: msg.payload[9]\n    }\n};\n\nreturn msg;","outputs":1,"noerr":0,"initialize":"","finalize":"","libs":[],"x":710,"y":280,"wires":[["d934aff363c2ead2"]]},{"id":"53700dcf95a61fe6","type":"modbus-flex-getter","z":"d6f7e07434fae5c3","name":"","showStatusActivities":false,"showErrors":false,"logIOActivities":false,"server":"afd5e2845d7decfa","useIOFile":false,"ioFile":"","useIOForPayload":false,"emptyMsgOnFail":false,"keepMsgProperties":false,"x":510,"y":440,"wires":[["7527b5fdb64f6132"],["9478fddd2959d993"]]},{"id":"d0f7193a0650840e","type":"modbus-flex-write","z":"d6f7e07434fae5c3","name":"","showStatusActivities":false,"showErrors":false,"server":"afd5e2845d7decfa","emptyMsgOnFail":false,"keepMsgProperties":false,"x":850,"y":520,"wires":[[],["b05172fdbc83e9af"]]},{"id":"cd0cdb6a22ed2a40","type":"function","z":"d6f7e07434fae5c3","name":"fc3 1[0~9]","func":"msg.payload = {\n    'fc': 3,\n    'unitid': 1,\n    'address': 0,\n    'quantity': 10\n};\nreturn msg;","outputs":1,"noerr":0,"x":320,"y":440,"wires":[["53700dcf95a61fe6"]]},{"id":"10f700938e9fb9f7","type":"inject","z":"d6f7e07434fae5c3","name":"","props":[{"p":"payload"},{"p":"topic","vt":"str"}],"repeat":"0.5","crontab":"","once":false,"onceDelay":0.1,"topic":"","payload":"","payloadType":"date","x":155,"y":440,"wires":[["cd0cdb6a22ed2a40"]]},{"id":"9478fddd2959d993","type":"modbus-response","z":"d6f7e07434fae5c3","name":"","registerShowMax":20,"x":740,"y":446,"wires":[]},{"id":"6b975682a2bbc647","type":"random","z":"d6f7e07434fae5c3","name":"","low":"0","high":"30","inte":"true","property":"payload","x":320,"y":520,"wires":[["0b45661d60566779"]]},{"id":"7cb838c943abe495","type":"function","z":"d6f7e07434fae5c3","name":"fc16 1[0~9]","func":"msg.payload = {\n    'value': msg.payload,\n    'fc': 16,\n    'unitid': 1,\n    'address': 0,\n    'quantity': 10\n};\nreturn msg;","outputs":1,"noerr":0,"initialize":"","finalize":"","libs":[],"x":650,"y":520,"wires":[["d0f7193a0650840e"]]},{"id":"0b45661d60566779","type":"join","z":"d6f7e07434fae5c3","name":"","mode":"custom","build":"array","property":"payload","propertyType":"msg","key":"topic","joiner":"\\n","joinerType":"str","accumulate":false,"timeout":"","count":"10","reduceRight":false,"reduceExp":"","reduceInit":"","reduceInitType":"","reduceFixup":"","x":490,"y":520,"wires":[["7cb838c943abe495"]]},{"id":"1cd641c99f86a519","type":"inject","z":"d6f7e07434fae5c3","name":"","props":[{"p":"payload"},{"p":"topic","vt":"str"}],"repeat":"0.1","crontab":"","once":false,"onceDelay":0.1,"topic":"","payload":"","payloadType":"date","x":160,"y":520,"wires":[["6b975682a2bbc647"]]},{"id":"b05172fdbc83e9af","type":"modbus-response","z":"d6f7e07434fae5c3","name":"","registerShowMax":20,"x":1070,"y":520,"wires":[]},{"id":"afd5e2845d7decfa","type":"modbus-client","name":"qwe","clienttype":"tcp","bufferCommands":false,"stateLogEnabled":false,"queueLogEnabled":false,"tcpHost":"192.168.68.91","tcpPort":"7777","tcpType":"DEFAULT","serialPort":"/dev/ttyAMA0","serialType":"RTU","serialBaudrate":"9600","serialDatabits":"8","serialStopbits":"1","serialParity":"none","serialConnectionDelay":"100","serialAsciiResponseStartDelimiter":"0x3A","unit_id":1,"commandDelay":1,"clientTimeout":1000,"reconnectOnTimeout":true,"reconnectTimeout":2000,"parallelUnitIdsAllowed":true}]
```

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221014.PNG)

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221015.PNG)

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221001.PNG)

>讀取 0 到 9 號 register，Modbus Flex Getter 節點是讀資料用的，讀到資料後，再用"函數"節點，把資料轉成 IoT Hub 的格式，傳入 IoT Hub

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221006.PNG)

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221003.PNG)

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221004.PNG)

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221005.PNG)

>生成 0 到 30 的數字，每 10 個數字組成一個陣列，寫入 0 到 9 號 register，Modbus Flex Write 節點是寫入資料用的

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221011.PNG)

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221012.PNG)

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221013.PNG)

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221007.PNG)

>用 Modbus Slave 模擬設備通訊

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221019.PNG)

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221020.PNG)

>可以再除錯窗口，看到資料成功寫入IoT Hub，跟讀取剛寫入 IoT Hub 的資料

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221016.PNG)

>用 Azure Function 把 IoT Hub 資料存入 PostgreSQL

```c#
    public void Run([IoTHubTrigger("messages/events", Connection = "Connectionstring")] EventData message, ILogger log)
    {
        try
        {
            string mes = $"{Encoding.UTF8.GetString(message.Body.Array).ToString()}";
            using (var db = new TestContext())
            {
                var test = new TestTable();
                test.Test = mes;
                db.Tests.Add(test);
                db.SaveChanges();
                log.LogInformation($"message: {Encoding.UTF8.GetString(message.Body.Array).ToString()}");
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.ToString());
        }
    }
```

>顯示所有資料

```sql
SELECT * FROM PUBLIC."Tests";
```

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221017.PNG)

>顯示 Json Key "a" 的資料最小、最大、總和、平均值

```sql
SELECT 
   MIN (CAST (PUBLIC."Tests"."Test" -> 'a' AS INTEGER)),
   MAX (CAST (PUBLIC."Tests"."Test" -> 'a' AS INTEGER)),
   SUM (CAST (PUBLIC."Tests"."Test" -> 'a' AS INTEGER)),
   AVG (CAST (PUBLIC."Tests"."Test" -> 'a' AS INTEGER))
FROM PUBLIC."Tests";
```

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221018.PNG)
