Several utility codes:
- to get user's public data from Twitter API
- to generate a graph for further visualization

## ติดตั้ง

- ทำสำเนาไฟล์ `config.ini-sample` และเปลี่ยนชื่อเป็น `config.ini`
- ในไฟล์ `config.ini` ใส่ข้อมูล token สำหรับเข้าถึง Twitter API
  
## ข้อมูล

ไฟล์ข้อมูลอยู่ใน Google Drive https://drive.google.com/drive/folders/1XQnJCc3xbTn6xFb-Vg4nJBtC-j1hC67c?usp=sharing (เฉพาะผู้ร่วมโครงการเท่านั้น)

## ขั้นตอนการเตรียมข้อมูล

### หารายการบัญชีตั้งต้นสำหรับผู้มีสิทธิ์เลือกตั้งครั้งแรก (first voters)

1. ใช้คำสำคัญที่น่าจะช่วยให้ระบุถึงผู้มีสิทธิ์เลือกตั้งครั้งแรก (เช่นแฮชแท็ก #เลือกตั้งครั้งแรก) ค้นหาข้อความในทวิตเตอร์ เพื่อให้ได้ชุดบัญชีตั้งต้น ที่น่าจะเป็นผู้มีสิทธิ์เลือกตั้งครั้งแรก
   - ขั้นตอนนี้ ใช้เครื่องมือ social media listening (Wisesight ZOCIAL EYE) ในการค้นหา สำหรับคำค้นนั้น ปรับแต่งด้วยมนุษย์
   - ได้มาประมาณ 8,000 บัญชี
   - อยู่ในไฟล์ [firstvoters-raw.xlsx](https://drive.google.com/file/d/1gctblwJsnsnvLT-IEPqUKlWn1wvQnzc5/view?usp=sharing)

2. นำรายชื่อบัญชีจาก (1) มาดึงข้อมูลเกี่ยวกับบัญชีนั้นๆ (มี follower กี่คน, following กี่คน, เริ่มเปิดบัญชีวันไหน, โพสต์มาแล้วกี่ข้อความ ฯลฯ)
   - ใช้โปรแกรม `get-users-info.ipynb`

3. กรองผู้ใช้ที่ไม่น่าจะใช่ผู้มีสิทธิ์เลือกตั้งครั้งแรกออกไป ทั้งการดูด้วยตาสำหรับบัญชีที่เห็นได้ชัด เช่น สำนักข่าว ดารา และการกรองด้วยเกณฑ์ เช่น ถ้าสมัครบัญชีทวิตเตอร์มาแล้วเกิน 12 ปี ก็ "น่าจะ" มีอายุเกิน 25 ปี (ทวิตเตอร์กำหนดให้สมัครบัญชีได้เมื่ออายุ 13 ปี) เป็นต้น   
   - กรองแล้วเหลือประมาณ 6,000 บัญชี
   - รายชื่อที่กรอง/จัดประเภทแล้ว อยู่ในไฟล์ [firstvoters-info-full.xlsx](https://drive.google.com/file/d/1ugUev3_Gefz3xk8V0m4-SqkX397rLQOo/view?usp=sharing)

### เก็บข้อมูลเครือข่ายของผู้มีสิทธิ์เลือกตั้งครั้งแรก

ขั้นนี้จะเป็นการหาว่าบัญชีต่างๆ มีการเชื่อมโยงกันหรือเชื่อมโยงขยายออกไปอย่างไรบ้าง

4. หาว่าบัญชีผู้มีสิทธิ์เลือกตั้งครั้งแรก ติดตามใคร (friends/following)
   - ใช้โปรแกรม	`get-friends-info.ipynb`
5. หาว่าบัญชีผู้มีสิทธิ์เลือกตั้งครั้งแรก มีใครมาติดตาม (followers)
   - ใช้โปรแกรม `get-followers-info.ipynb`

หนึ่งบัญชีอาจไปติดตามบัญชีอื่นๆ หรือมีผู้มาติดตาม หลักร้อย พัน หรือหมื่นบัญชี ดังนั้นในขั้นนี้ จำนวนบัญชีจะขยายออกไปอย่างมาก

เพื่อเป็นการจำกัดจำนวนข้อมูลในอยู่ในระดับที่จัดการได้ อาจทำการสุ่มบัญชีจากรายชื่อที่ได้จาก (3) อีกที เพื่อให้มีจำนวนน้อยลง
- ดูบัญชีที่สุ่มขึ้นมา 116 บัญชี ได้ใน [Google Sheet](https://docs.google.com/spreadsheets/d/1bTCMhcB5Iju4RQUCz4mnhxsxhN3nOLsN6PZ133qIDuA/edit#gid=327442729) (export ออกมาเป็นไฟล์ [firstvoters-info-samples-116-with-labels.xlsx](https://drive.google.com/file/d/1HsmeDoTrJFEGx2rcrtS8DJQAz_WknYw-/view?usp=sharing))
- จะใช้ 116 บัญชีนี้ เป็นขุดบัญชีตั้งต้น (seed) ในการหา friends และ followers ในขั้นที่ (4) และ (5)

### สร้างกราฟจากข้อมูลเครือข่าย

6. รวมบัญชีที่ได้จาก (4) และ (5) เข้าด้วยกัน
   - ใช้โปรแกรม `combine-nodes.ipynb`

7. สร้างกราฟข้อมูลเครือข่ายที่เก็บมาได้จาก (4) และ (5) และได้รวมเข้าด้วยกันในขั้นตอนที่ (6) แล้ว
	 - ใช้โปรแกรม `create-user-graph.ipynb`
   - จะได้ไฟล์นามสกุล `.graphml` ซึ่งสามารถนำไปเปิดในโปรแกรม [Gephi](https://gephi.org/) ได้

8. ข้อมูลเครือข่ายนี้ จะทำให้เราสามารถหาบัญชีที่น่าจะเป็น "แกนกลาง" หรือมีความสำคัญสำหรับเครือข่ายนี้ได้
   - มีวิธีหา "แกนกลาง" นี้ได้หลายวิธี วิธีหนึ่งคือ PageRank
   - การหาแกนกลางนี้ จะช่วยลดจำนวนบัญชีที่จำเป็นต้องติดป้ายกำกับ เช่น แทนที่จะต้องติดป้ายกำกับกับบัญชีทุกบัญชี (ซึ่งอาจมีเป็นหมื่นบัญชี) เราอาจติดป้ายกำกับเฉพาะบัญชีที่อยู่ 1,000 อันดับแรกของ PageRank 

### ดึงข้อความโพสต์จากบัญชีเพื่อติดป้ายกำกับและหาเครือข่ายผ่านการ Retweet และ @reply

9. เพื่อให้สามารถติดป้ายกำกับได้เร็วขึ้น เราอาจดึงข้อความจำนวนมากด้วยคอมพิวเตอร์ ซึ่งอาจเลือกใช้วิธีเหล่านี้
   1. ดึงข้อความของบัญชีนั้นๆ จากเครื่องมือ social media listening
   2. ใช้โปรแกรม `get-users-msg-api.ipynb`

10. การหาเครือข่ายผ่านการ Retweet

11. การหาเครือข่ายผ่านการ @reply หรือ @mention
