# 깃 허브 올리기 과정
- - -

1. git init

2. git add . 
   현재 수정된 모든 파일들을 올리기 위해 스토리지로 옮길 준비.
   
3. git commit -m "Message"

4. git push origin +master 
   origin이라는 브랜치로 파일 전송
   
5. npm run deploy
   그전에 Package.json 파일에서 predeploy와 deploy, homepage 를 설정해야 한다.
