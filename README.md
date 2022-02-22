# Modbus 資料傳到 IoT-Hub 再存入 PostgreSQL

* [測試環境](#測試環境)
* [Install](#Install)
* [範例](#範例)

<h1 id="測試環境"> 測試環境 </h1>

Raspberry Pi 4 Model B 4GB

<h1 id="Install"> Install </h1>

## 以下操作在 Raspberry Pi 執行

開啟 Raspberry Pi terminal

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221040.png)

檢查有沒有安裝 Node.js，有安裝的話會出現版本號

```
node -v
```

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221021.PNG)

執行 `node -v` 出現 `Command 'node' not found` 表示沒有安裝 Node.js，執行以下指令安裝 Node.js

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221023.PNG)

```
sudo apt-get install nodejs-legacy
```

檢查有沒有安裝 npm，有安裝的話會出現版本號

```
npm -v
```

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221022.PNG)

執行 `npm -v` 出現 `Command 'npm' not found` 表示沒有安裝 npm，執行以下指令安裝 npm

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221024.PNG)

```
sudo apt-get install npm
```

安裝 Node-RED

```
sudo npm install -g --unsafe-perm node-red node-red-admin 
```

啟動 Node-RED service

```
node-red-start
```

啟動後，打開瀏覽器輸入樹梅派 IP，Port 1880 就可以進入

```
http://xxx.xxx.xxx.xxx:1880
```

停止 Node-RED service

```
node-red-stop
```

假設部屬完後 service 掛掉，可以進入安全模式把有衝突或打錯的東西改掉再進行一次部屬(以建立到 2 個一樣 port 的 Modbus Server 為例)

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221025.PNG)

```
node-red --safe
```

設定開機自動啟動 Node-RED service

```
sudo systemctl enable nodered.service
```

<h1 id="範例"> 範例 </h1>

## 1. 安裝必要套件

打開瀏覽器輸入樹梅派 IP，Port 1880 就可以進入 Node-RED

```
http://xxx.xxx.xxx.xxx:1880
```

進入 Node-RED 後，點擊畫面右上角 ![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221027.PNG)，再點擊設置

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221026.PNG)

開啟設置後，1. 點擊 Palette，2. 點擊安裝，3. 在搜尋列輸入 `modbus`，4. 找到 `node-red-contrib-modbus` 點擊安裝

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221009-3.PNG)

在搜尋列輸入 `iot-hub`，找到 `node-red-contrib-azure-iot-hub` 點擊安裝

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221010-2.PNG)

## 2. 匯入範例

在 Node-RED，點擊畫面右上角 ![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221027.PNG)，再點擊匯入

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221028.PNG)

複製以下 Json，貼入剪貼簿，再點擊匯入

```Json
[{"id":"d6f7e07434fae5c3","type":"tab","label":"F2","disabled":false,"info":"","env":[]},{"id":"f775e252.a49f2","type":"debug","z":"d6f7e07434fae5c3","name":"Log","active":true,"console":"false","complete":"true","x":1170,"y":280,"wires":[]},{"id":"817f33a3.ddf5f","type":"azureiothubreceiver","z":"d6f7e07434fae5c3","name":"Azure IoT Hub Receiver","x":880,"y":340,"wires":[["c2825fc8.d6323"]]},{"id":"c2825fc8.d6323","type":"debug","z":"d6f7e07434fae5c3","name":"Log","active":true,"console":"false","complete":"true","x":1170,"y":340,"wires":[]},{"id":"d934aff363c2ead2","type":"azureiothub","z":"d6f7e07434fae5c3","name":"Azure IoT Hub","protocol":"mqtt","x":900,"y":280,"wires":[["f775e252.a49f2"]]},{"id":"7527b5fdb64f6132","type":"function","z":"d6f7e07434fae5c3","name":"","func":"msg.payload = {\n\tdeviceId: \"test777\",\n    key: \"aUecqAb3OJ2CNak4MU5X9pGES2QCumQs2L0XEb2DNPM=\",\n    protocol: \"mqtt\",\n    data: {\n        a: msg.payload[0],\n        b: msg.payload[1],\n        c: msg.payload[2],\n        d: msg.payload[3],\n        e: msg.payload[4],\n        f: msg.payload[5],\n        g: msg.payload[6],\n        h: msg.payload[7],\n        i: msg.payload[8],\n        j: msg.payload[9]\n    }\n};\n\nreturn msg;","outputs":1,"noerr":0,"initialize":"","finalize":"","libs":[],"x":710,"y":280,"wires":[["d934aff363c2ead2"]]},{"id":"53700dcf95a61fe6","type":"modbus-flex-getter","z":"d6f7e07434fae5c3","name":"","showStatusActivities":false,"showErrors":false,"logIOActivities":false,"server":"109d23776188deaa","useIOFile":false,"ioFile":"","useIOForPayload":false,"emptyMsgOnFail":false,"keepMsgProperties":false,"x":510,"y":440,"wires":[["7527b5fdb64f6132"],["9478fddd2959d993"]]},{"id":"d0f7193a0650840e","type":"modbus-flex-write","z":"d6f7e07434fae5c3","name":"","showStatusActivities":false,"showErrors":false,"server":"109d23776188deaa","emptyMsgOnFail":false,"keepMsgProperties":false,"x":850,"y":520,"wires":[[],["b05172fdbc83e9af"]]},{"id":"cd0cdb6a22ed2a40","type":"function","z":"d6f7e07434fae5c3","name":"fc3 1[0~9]","func":"msg.payload = {\n    'fc': 3,\n    'unitid': 1,\n    'address': 0,\n    'quantity': 10\n};\nreturn msg;","outputs":1,"noerr":0,"x":320,"y":440,"wires":[["53700dcf95a61fe6"]]},{"id":"10f700938e9fb9f7","type":"inject","z":"d6f7e07434fae5c3","name":"","props":[{"p":"payload"},{"p":"topic","vt":"str"}],"repeat":"0.1","crontab":"","once":false,"onceDelay":0.1,"topic":"","payload":"","payloadType":"date","x":155,"y":440,"wires":[["cd0cdb6a22ed2a40"]]},{"id":"9478fddd2959d993","type":"modbus-response","z":"d6f7e07434fae5c3","name":"","registerShowMax":20,"x":740,"y":446,"wires":[]},{"id":"6b975682a2bbc647","type":"random","z":"d6f7e07434fae5c3","name":"","low":"0","high":"30","inte":"true","property":"payload","x":320,"y":520,"wires":[["0b45661d60566779"]]},{"id":"7cb838c943abe495","type":"function","z":"d6f7e07434fae5c3","name":"fc16 1[0~9]","func":"msg.payload = {\n    'value': msg.payload,\n    'fc': 16,\n    'unitid': 1,\n    'address': 0,\n    'quantity': 10\n};\nreturn msg;","outputs":1,"noerr":0,"initialize":"","finalize":"","libs":[],"x":650,"y":520,"wires":[["d0f7193a0650840e"]]},{"id":"0b45661d60566779","type":"join","z":"d6f7e07434fae5c3","name":"","mode":"custom","build":"array","property":"payload","propertyType":"msg","key":"topic","joiner":"\\n","joinerType":"str","accumulate":false,"timeout":"","count":"10","reduceRight":false,"reduceExp":"","reduceInit":"","reduceInitType":"","reduceFixup":"","x":490,"y":520,"wires":[["7cb838c943abe495"]]},{"id":"1cd641c99f86a519","type":"inject","z":"d6f7e07434fae5c3","name":"","props":[{"p":"payload"},{"p":"topic","vt":"str"}],"repeat":"0.1","crontab":"","once":false,"onceDelay":0.1,"topic":"","payload":"","payloadType":"date","x":160,"y":520,"wires":[["6b975682a2bbc647"]]},{"id":"b05172fdbc83e9af","type":"modbus-response","z":"d6f7e07434fae5c3","name":"","registerShowMax":20,"x":1070,"y":520,"wires":[]},{"id":"4ef41e123d23f75a","type":"modbus-server","z":"d6f7e07434fae5c3","name":"","logEnabled":false,"hostname":"0.0.0.0","serverPort":"7777","responseDelay":100,"delayUnit":"ms","coilsBufferSize":10000,"holdingBufferSize":10000,"inputBufferSize":10000,"discreteBufferSize":10000,"showErrors":false,"x":520,"y":160,"wires":[[],[],[],[],[]]},{"id":"109d23776188deaa","type":"modbus-client","name":"777","clienttype":"tcp","bufferCommands":true,"stateLogEnabled":false,"queueLogEnabled":false,"tcpHost":"127.0.0.1","tcpPort":"7777","tcpType":"DEFAULT","serialPort":"/dev/ttyUSB","serialType":"RTU-BUFFERD","serialBaudrate":"9600","serialDatabits":"8","serialStopbits":"1","serialParity":"none","serialConnectionDelay":"100","serialAsciiResponseStartDelimiter":"0x3A","unit_id":"1","commandDelay":"1","clientTimeout":"1000","reconnectOnTimeout":true,"reconnectTimeout":"2000","parallelUnitIdsAllowed":true}]
```

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221015.PNG)

範例成功匯入

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221029.PNG)

範例的 Modbus Server 以本地 port 7777 為例(可以按需求做更改)

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221032.PNG)

## 3. IoT Hub 節點設定

IoT Hub 節點

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221036.png)

function 節點要填入 IoT Hub device 的 deviceId 跟 device primary key

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221039.PNG)

Azure IoT Hub 節點要填入 IoT Hub 的 hostname，使資料可以依前面 function 節點設定傳入 IoT Hub

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221037.PNG)

Azure IoT Hub Receiver 節點要填入 IoT Hub 的 connection string，接收傳入 IoT Hub 的資料

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221038.PNG)

## 4. Modbus 寫入功能

Modbus 寫入功能的節點

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221030.PNG)

inject 節點，設定 0.1秒 poll 一次

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221011.PNG)

random 節點，生成 0 到 30 的數字

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221012.PNG)

join 節點，接前面 random 節點生成的數字，再以每 10 個數字組成一個陣列

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221013.PNG)

function 節點，設定 Function Code、Slave Id、register 起始位址、資料筆數，Modbus Flex Write 節點接到前面 function 節點設定後將資料寫入(以寫入 0 到 9 號 register 為例)

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221007.PNG)

## 5. Modbus 讀取功能

Modbus 讀取功能的節點

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221031.PNG)

function 節點，設定 Function Code、Slave Id、register 起始位址、資料筆數，Modbus Flex Getter 節點接到前面 function 節點設定後讀取資料(以讀取 0 到 9 號 register 為例)

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221006.PNG)

Modbus Flex Getter 節點連接 function 節點，把資料轉成 IoT Hub 節點要的格式，傳入 IoT Hub

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221005.PNG)

點擊畫面右上角的 ![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221035.PNG) 進入除錯窗口，可以看到資料成功寫入 IoT Hub，跟讀取剛寫入 IoT Hub 的資料

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221034.PNG)

## 6. 用 Azure Function 把上面 Node-RED 寫入 IoT Hub 的資料存入 PostgreSQL

範例程式

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

## 7. PostgreSQL Query 範例

顯示所有資料

```sql
SELECT * FROM PUBLIC."Tests";
```

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221017.PNG)

顯示 Json Key "a" 的資料最小、最大、總和、平均值

```sql
SELECT 
   MIN (CAST (PUBLIC."Tests"."Test" -> 'a' AS INTEGER)),
   MAX (CAST (PUBLIC."Tests"."Test" -> 'a' AS INTEGER)),
   SUM (CAST (PUBLIC."Tests"."Test" -> 'a' AS INTEGER)),
   AVG (CAST (PUBLIC."Tests"."Test" -> 'a' AS INTEGER))
FROM PUBLIC."Tests";
```

![](https://github.com/Little-Y8763/Modbus_IoT-Hub_PostgreSQL/blob/main/Doc/picture/20220221018.PNG)
