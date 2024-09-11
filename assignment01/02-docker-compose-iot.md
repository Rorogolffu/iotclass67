# IoT Docker compose
>> อธิบายโครงสร้าง docker-compose.yml สำหรับเซอร์วิสต่าง ๆ ที่เกี่ยวข้องกับระบบการสตรีมข้อมูลแบบเรียลไทม์โดยใช้ Kafka, Prometheus และ Grafana:

ZooKeeper
ZooKeeper เป็นบริการที่ใช้สำหรับการจัดเก็บข้อมูล configuration แบบกระจายและการซิงโครไนซ์ที่เชื่อถือได้ระหว่างระบบหลายตัว นอกจากนี้ยังให้บริการจัดการกลุ่มใน cluster Kafka

พอร์ต: 2181
ข้อมูล: เก็บในโฟลเดอร์ /var/lib/zookeeper
Kafka
Kafka เป็น platform การ streaming ข้อมูลแบบกระจายที่ใช้สำหรับการสร้าง pipeline ข้อมูลแบบเรียลไทม์ รวมถึงการพัฒนาแอปพลิเคชันที่ตอบสนองต่อข้อมูลในทันที

การเชื่อมต่อกับ ZooKeeper: ใช้พอร์ต 9092
คุณสมบัติ: รองรับการบีบอัดข้อมูลและการสร้างหัวข้อ (topics) อัตโนมัติ
Kafka REST Proxy
Kafka REST Proxy ให้การเชื่อมต่อแบบ RESTful กับ Kafka ทำให้ง่ายต่อการจัดการข้อความ รวมถึงการดูสถานะของ cluster Kafka โดยไม่ต้องใช้ Kafka protocol

พอร์ต: 8082
การเชื่อมต่อกับ ZooKeeper: ใช้พอร์ต 2181
Kafka Topics UI Interface สำหรับการเรียกดูและจัดการ (topics) ของ Kafka

พอร์ต: 8081
เชื่อมต่อกับ Kafka REST Proxy
Kafka Connect
เป็นเฟรมเวิร์กสำหรับเชื่อมต่อ Kafka กับระบบภายนอก เช่น database, valuestores หรือไฟล์

พอร์ต: 8083
ใช้ ZooKeeper สำหรับการจัดการคลัสเตอร์
Kafka Connect UI
Interface สำหรับการจัดการ Kafka Connect

พอร์ต: 8082
เชื่อมต่อกับ Kafka Connect
Mosquitto (MQTT Broker)
เป็น MQTT Broker แบบ Opensource ที่รองรับ Protocol MQTT สำหรับการสื่อสารแบบ publish/subscribe ซึ่งเหมาะกับอุปกรณ์ IoT

พอร์ต: 1883
MongoDB
ฐานข้อมูลแบบ NoSQL ที่เก็บข้อมูลแบบเอกสาร

พอร์ต: 27017
ผู้ใช้และรหัสผ่าน: ตั้งค่าผ่าน .env ไฟล์
Mongo Express
Web Interface สำหรับจัดการ MongoDB

พอร์ต: 8084
เชื่อมต่อกับ MongoDB
Grafana
Platform แสดงผลข้อมูลที่ใช้สำหรับสร้าง Dashboard เพื่อติดตามและแสดงผลข้อมูลแบบ Realtime

พอร์ต: 8085
ปลั๊กอิน: รองรับการติดตั้ง Plugin เพิ่มเติม เช่น Grafana Clock, Worldmap, และ Piechart
Prometheus
ระบบที่ใช้สำหรับเก็บข้อมูล Metric ในฐานข้อมูลแบบ time-series และสร้างการแจ้งเตือนตามการเปลี่ยนแปลงของ Metric เหล่านั้น

พอร์ต: 8086
เก็บข้อมูลใน: /etc/prometheus/
Node Exporter
ส่งออกข้อมูลเมตริกเกี่ยวกับเครื่องที่ใช้งาน (เซิร์ฟเวอร์) ให้กับ Prometheus

พอร์ต: 9100
แต่ละ Service จะทำงานร่วมกันเพื่อสร้าง Environment ในการ streaming ข้อมูลและการตรวจสอบข้อมูลแบบ realtime ทั้งนี้การเชื่อมต่อระหว่าง service ใช้พอร์ตต่าง ๆ และการตั้งค่าเหล่านี้สามารถปรับแต่งได้ตามความต้องการ



## start-service #0
>> หน้าจอที่ 1:

    zookeeper: ใช้สำหรับจัดการการตั้งค่าการกระจาย, การตั้งชื่อ, การซิงโครไนซ์การกระจาย และการให้บริการกลุ่มในคลัสเตอร์ Kafka.
    kafka: ทำหน้าที่เป็นแพลตฟอร์มสตรีมมิ่งที่กระจาย ซึ่งใช้สร้างท่อข้อมูลเรียลไทม์ และแอปพลิเคชันการสตรีมมิ่งที่ตอบสนองต่อข้อมูล.
    kafka-rest-proxy: ให้บริการ RESTful interface สำหรับ Kafka cluster เพื่อให้สามารถผลิตและบริโภคข้อความได้ง่ายขึ้น.
    kafka-topics-ui: เครื่องมือที่ใช้สำหรับดูและจัดการ Kafka topics ผ่าน UI.
    kafka-connect: เป็นเฟรมเวิร์คที่ใช้เชื่อมต่อ Kafka กับระบบภายนอก เช่น ฐานข้อมูลและไฟล์ระบบ.
    kafka-connect-ui: UI สำหรับการจัดการและตั้งค่าการเชื่อมต่อใน Kafka Connect.

## start-service #1
>> หน้าจอที่ 2:

    mongo: ฐานข้อมูล MongoDB สำหรับจัดเก็บข้อมูล.
    mongo-express: เว็บอินเตอร์เฟซสำหรับจัดการ MongoDB, ใช้งานง่ายสำหรับการดูและจัดการข้อมูลใน MongoDB.
    mosquitto: MQTT broker สำหรับการสื่อสารแบบ publish/subscribe ระหว่างอุปกรณ์ IoT.
    prometheus: ระบบการตรวจสอบและการแจ้งเตือนที่บันทึกเมตริกเวลาจริงในฐานข้อมูลซีรีส์เวลา.
    nodeexporter: ใช้สำหรับการส่งออกเมตริกของเครื่องที่ Prometheus.

## start-service #2
>> หน้าจอที่ 3:

    grafana: เครื่องมือสำหรับการแสดงผลและการวิเคราะห์ข้อมูลผ่านแดชบอร์ด, ใช้เพื่อแสดงข้อมูลจาก Prometheus และอื่นๆ.
    pushgateway: ใช้สำหรับส่งข้อมูลจากงานที่ใช้เวลานาน (long-running jobs) ไปยัง Prometheus.

## start-service #3
>> หน้าจอที่ 4:

    iot_sensor_1, iot_sensor_2, iot_sensor_3: เซนเซอร์ IoT ที่ใช้ส่งข้อมูลไปยังระบบ.
    iot-processor: โปรเซสเซอร์สำหรับประมวลผลข้อมูลที่มาจากเซนเซอร์ IoT.
