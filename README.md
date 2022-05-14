# SSR react legacy Suggestion 💡
* ### Structure code <br/>
การวางโครงสร้างของProjectควรแยกFolderให้ละเอียดกว่านี้เพื่อให้codeเป็นระเบียบและสามารถอ่านหรือแก้ไขได้ง่ายมากยิ่งขึ้นโดยเฉพาะcodeในส่วนของstate managementควรที่จะสร้างfolder storeขึ้นมาเพื่อเก็บstateโดยเฉพาะและในfolder storeก็ควรที่จะสร้างไฟล์action,reducer,selectorแยกกันเพื่อให้ง่ายต่อการพัฒนาในอนาคต
* ### การตั้งชื่อ  <br/>
ควรตั้งชื่อไฟล์บางไฟล์ให้สอดคล้องกับfunctionที่เขียน
* ### Console.log  <br/>
ไม่ควรมีconsole.logและcommentในcode
* ### Test  <br/>
ควรเขียนUnit testเพื่อเช็คความถูกต้องของfunctionที่เราเขียน
* ### CI/CD pipeline  <br/>
ควรทำCI/CD pipelineเพื่อช่วยเพิ่มประสิทธิภาพในการเขียนcode