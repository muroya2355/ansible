# 構成

C:\Users\user\Desktop\shared\terasoluna-tourreservation\terasoluna-tourreservation-env\src\main\resources\META-INF\spring\terasoluna-tourreservation-infra.properties
⇒
database.url=jdbc:postgresql://192.168.128.132/tourreserve
database.username=appuser
database.password=password


‪C:\Users\user\Desktop\shared\terasoluna-tourreservation-5.5.1.RELEASE\terasoluna-tourreservation-initdb\pom.xml

C:\Users\user\Desktop\shared\terasoluna-tourreservation-5.5.1.RELEASE\terasoluna-tourreservation-env\configs\tomcat9-postgresql\ContainerConfigXML\context.xml

C:\Users\user\Desktop\shared\terasoluna-tourreservation-5.5.1.RELEASE\terasoluna-tourreservation-env\configs\tomcat-postgresql\ContainerConfigXML\context.xml

<!--
C:\Users\user\Desktop\shared\terasoluna-tourreservation-5.5.1.RELEASE\terasoluna-tourreservation-env\configs\tomcat85-postgresql\ContainerConfigXML

C:\Users\user\Desktop\shared\terasoluna-tourreservation-5.5.1.RELEASE\terasoluna-tourreservation-env\configs\tomcat8-postgresql\ContainerConfigXML
-->

scp -r /mnt/hgfs/shared/terasoluna-tourreservation-5.5.1.RELEASE root@192.168.153.131:~/

ssh root@192.168.153.131
ls
cd terasoluna-tourreservation-5.5.1.RELEASE/

mvn -f terasoluna-tourreservation-initdb/pom.xml sql:execute
mvn clean install
ls terasoluna-tourreservation-web/target/terasoluna-tourreservation-web.war

cp -ipv terasoluna-tourreservation-web/target/terasoluna-tourreservation-web.war /opt/tomcat/webapps/
ls /opt/tomcat/webapps/

http://192.168.128.131/terasoluna-tourreservation-web/
⇒ ツアー検索する
⇒ 北海道→北海道 で検索

