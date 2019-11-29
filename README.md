![Network of Twitter first voters (samples)](samples116-graph.png "Network of Twitter first voters (samples)")

# EDANG

Several utility codes:
- to get user's public data from Twitter API
- to generate a GraphML file for visualization

## ติดตั้ง

- ทำสำเนาไฟล์ `config.ini-sample` และเปลี่ยนชื่อเป็น `config.ini`
- ในไฟล์ `config.ini` ใส่ข้อมูล token สำหรับเข้าถึง Twitter API
- ดูวิธีการขอ/ดู token ที่ [Twitter Developers](https://developer.twitter.com/en/docs/basics/authentication/guides/access-tokens)
  
## ข้อมูล

ไฟล์ข้อมูลอยู่ใน Google Drive https://drive.google.com/drive/folders/1XQnJCc3xbTn6xFb-Vg4nJBtC-j1hC67c?usp=sharing (เฉพาะผู้ร่วมโครงการเท่านั้น)

## ขั้นตอนการเตรียมข้อมูล

### หารายการบัญชีตั้งต้นสำหรับผู้มีสิทธิ์เลือกตั้งครั้งแรก (first voters)

1. ใช้คำสำคัญที่น่าจะช่วยให้ระบุถึงผู้มีสิทธิ์เลือกตั้งครั้งแรก (เช่นแฮชแท็ก #เลือกตั้งครั้งแรก) ค้นหาข้อความในทวิตเตอร์ เพื่อให้ได้ชุดบัญชีตั้งต้น ที่น่าจะเป็นผู้มีสิทธิ์เลือกตั้งครั้งแรก
   - ขั้นตอนนี้ ใช้เครื่องมือ social media listening (Wisesight ZOCIAL EYE) ในการค้นหา สำหรับคำค้นนั้น ปรับแต่งด้วยมนุษย์
   - ได้มาประมาณ 8,000 บัญชี
   - อยู่ในไฟล์ [firstvoters-full-raw.xlsx](https://drive.google.com/file/d/1DO5hdhSNjP-zNpXg4HSkCT64sSHY6pcH/view?usp=sharing)

2. นำรายชื่อบัญชีจาก (1) มาดึงข้อมูลเกี่ยวกับบัญชีนั้นๆ (มี follower กี่คน, following กี่คน, เริ่มเปิดบัญชีวันไหน, โพสต์มาแล้วกี่ข้อความ ฯลฯ)
   - ใช้โปรแกรม `get-users-info.ipynb`

3. กรองผู้ใช้ที่ไม่น่าจะใช่ผู้มีสิทธิ์เลือกตั้งครั้งแรกออกไป ทั้งการดูด้วยตาสำหรับบัญชีที่เห็นได้ชัด เช่น สำนักข่าว ดารา และการกรองด้วยเกณฑ์ เช่น ถ้าสมัครบัญชีทวิตเตอร์มาแล้วเกิน 12 ปี ก็ "น่าจะ" มีอายุเกิน 25 ปี (ทวิตเตอร์กำหนดให้สมัครบัญชีได้เมื่ออายุ 13 ปี) เป็นต้น   
   - กรองแล้วเหลือประมาณ 6,000 บัญชี
   - รายชื่อที่กรอง/จัดประเภทแล้ว อยู่ในไฟล์ [firstvoters-full-info.xlsx](https://drive.google.com/file/d/1qwJaB5nIdggT2I6SdQxy5sudbn5yj85F/view?usp=sharing)

เพื่อเป็นการจำกัดจำนวนข้อมูลในอยู่ในระดับที่จัดการได้ จึงทำการสุ่มบัญชีจากรายชื่อที่ได้จาก (3) อีกที เพื่อให้มีจำนวนน้อยลง
- ดูบัญชีที่สุ่มขึ้นมา 116 บัญชี ได้ใน [Google Sheet](https://docs.google.com/spreadsheets/d/1bTCMhcB5Iju4RQUCz4mnhxsxhN3nOLsN6PZ133qIDuA/edit?usp=sharing) (export เมื่อวันที่ 10 พ.ย. 2562 ออกมาเป็นไฟล์ [samples116-info-with-labels.xlsx](https://drive.google.com/file/d/1BEznhsNE26vNlL4s1f04xG6exujzHtW5/view?usp=sharing))
- จะใช้ 116 บัญชีนี้ เป็นขุดบัญชีตั้งต้น (seed) ในการหา friends และ followers ในขั้นที่ (4) และ (5)

### เก็บข้อมูลเครือข่ายของผู้มีสิทธิ์เลือกตั้งครั้งแรก

ขั้นนี้จะเป็นการหาว่าบัญชีต่างๆ มีการเชื่อมโยงกันหรือเชื่อมโยงขยายออกไปอย่างไรบ้าง

4. หาว่าบัญชีผู้มีสิทธิ์เลือกตั้งครั้งแรก ติดตามใคร (friends/following)
   - ใช้โปรแกรม	`get-friends-info.ipynb`
   - ข้อมูลที่ได้มาจะเป็น array ของ User object (Tweepy), ดูตัวอย่างได้จากไฟล์ Pickle - [samples116-friends.pkl](https://drive.google.com/file/d/1ZN3tBx5jyHoTM4Bmb6IO2RlpvHBgsPk7/view?usp=sharing)

5. หาว่าบัญชีผู้มีสิทธิ์เลือกตั้งครั้งแรก มีใครมาติดตาม (followers)
   - ใช้โปรแกรม `get-followers-info.ipynb`
   - ข้อมูลที่ได้มาจะเป็น array ของ User object (Tweepy), ดูตัวอย่างได้จากไฟล์ Pickle - [samples116-followers.pkl](https://drive.google.com/file/d/1oth3lTlInj--eUtPlhS0hI3DNjwu4s0o/view?usp=sharing)

หนึ่งบัญชีอาจไปติดตามบัญชีอื่นๆ หรือมีผู้มาติดตาม หลักร้อย พัน หรือหมื่นบัญชี ดังนั้นในสองขั้นนี้ จำนวนบัญชีจะขยายออกไปอย่างมาก
(จากตัวอย่าง 116 บัญชีตั้งต้น ขยายเป็น 75,961 บัญชี (nodes) และมีความสัมพันธ์เชื่อมโยง (edges) 92,327 เส้น)

### สร้างกราฟจากข้อมูลเครือข่าย

6. สร้างกราฟข้อมูลเครือข่ายที่เก็บมาได้จาก (4) และ (5) และหา PageRank
   - ใช้โปรแกรม `create-user-graph.ipynb` เพื่อสร้างกราฟข้อมูลเครือข่าย
     - ตัวอย่างกราฟข้อมูลเครือข่าย ซึ่งมีจำนวนบัญชี 75,961 บัญชี (จากตัวอย่างบัญชีตั้งต้น 116 บัญชี): [samples116-graph.pkl](https://drive.google.com/file/d/1_my2K01lBAcFOVzmH_Vw9r-yfRrqbeYA/view?usp=sharing)
       - เก็บในรูปแบบ Pickle ของ Python. โครงสร้างข้อมูลเป็น dictionary ที่มี key เป็น screen name และ value เป็น Tweepy User object
   - จากกราฟนี้ เราสามารถหาบัญชีที่น่าจะเป็น "แกนกลาง" หรือมีความสำคัญสำหรับเครือข่ายนี้ได้ มีวิธีหา "แกนกลาง" นี้ได้หลายวิธี วิธีหนึ่งคือ PageRank
     - **รายชื่อบัญชี 1,200 อันดับแรกจาก PageRank เมื่อใช้ข้อมูลจากกราฟเครือข่ายบัญชีตั้งต้น 116 บัญชี: [samples116-pagerank-top1200.txt](https://drive.google.com/file/d/11DymC9z0lxPNNkhu9vvEwwbnNGA6Hocz/view?usp=sharing)**
      - การหาแกนกลางนี้ จะช่วยลดจำนวนบัญชีที่จำเป็นต้องติดป้ายกำกับ เช่น แทนที่จะต้องติดป้ายกำกับกับบัญชีทุกบัญชี (ซึ่งอาจมีเป็นหมื่นบัญชี) เราอาจติดป้ายกำกับเฉพาะบัญชีที่อยู่ 1,000 อันดับแรกของ PageRank
   - ข้อมูลเครือข่ายนี้ สามารถนำไปวาดเป็นแผนภูมิได้
     - ดูตัวอย่างไฟล์ GraphML (สร้างจาก seed จำนวน 96 บัญชี) ที่ https://github.com/bact/edang/blob/master/samples.graphml     
     - ไฟล์ GraphML (นามสกุล `.graphml`) นี้สามารถนำไปเปิดในโปรแกรมเช่น [Gephi](https://gephi.org/) หรือ [visone](http://visone.info) ได้

### ดึงข้อความโพสต์จากบัญชีเพื่อติดป้ายกำกับและหาเครือข่ายผ่านการ Retweet และ @reply

7. เพื่อให้สามารถติดป้ายกำกับได้เร็วขึ้น เราอาจดึงข้อความจำนวนมากด้วยคอมพิวเตอร์ ซึ่งอาจเลือกใช้วิธีเหล่านี้
   1. ดึงข้อความของบัญชีนั้นๆ จากเครื่องมือ social media listening
   2. ใช้โปรแกรม `get-users-msg-api.ipynb`
     - ข้อความที่ดึงมาด้วย `get-users-msg-api.ipynb` จะถูกเก็บเป็นไฟล์ข้อความ 1 บัญชีทวิตเตอร์ต่อ 1 ไฟล์
     - **ข้อความทวีตล่าสุด 1,200 ข้อความของบัญชี Top 1,2000 PageRank (ย้อนไปถึงวันที่ 1 ธ.ค. 2561) อยู่ในโฟลเดอร์ [samples116-pagerank-top1200](https://drive.google.com/drive/folders/1zlQw9MaOAcjTe6dDASKYhJ4gCTN9muG9?usp=sharing) ใน Google Drive**

8. การหาเครือข่ายผ่านการ Retweet

9. การหาเครือข่ายผ่านการ @reply หรือ @mention
