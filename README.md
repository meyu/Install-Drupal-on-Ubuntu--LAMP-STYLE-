所為何來
=
以 Drupal 搭配 Ubuntu、Apache、MySQL、PHP 來架設網站 (即 LAMP)。  
本文使用 Ubuntu 的 Drupal-LAMP 套裝指令，能簡單又快速地完成架設；但皆採用預設值安裝，在安全性及效能上較難盡人意，僅推薦做為短期服務的網站；欲架設網站長期經營者，不推薦使用此方法，或請自行微調各項設定。

適用環境
=
作業系統：Ubuntu 12.04 (Server & Desktop)  
網頁伺服器：Apache 2.2  
資料庫系統：MySQL 5.5  
指令碼語言：PHP 5.3  
資料庫管理介面：phpMyAdmin 4  
內容管理系統：Drupal 7.23

安裝方式
=
###進行 LAMP 架構的安裝及設定
於終端機中，輸入以下指令來更新 Ubuntu 並安裝 Drupal 所需之伺服器元件：
```bash
sudo aptitude update && sudo aptitude upgrade &&
sudo aptitude install drupal7
```
安裝過程中，請設定 MySQL 的 root (最高權限管理者) 的密碼：  
(本文使用 <code>root.password</code> 做為 root 的密碼)
![Set Password for MySQL root](https://lh5.googleusercontent.com/-C7DmC1wJ5RY/Ugxmh7Rh29I/AAAAAAAAf48/Q6kToP6w0YQ/w850-h567-no/01+Set+Password+for+MySQL+root.jpg)

設定電子郵件伺服器：  
(請依需求自行設定，本文選擇不設定)
![Choose No Configuration for Mail Service](https://lh3.googleusercontent.com/-Q_BFwKU3lHc/Ugxmhx_Do5I/AAAAAAAAf44/vzDUo7Y1jDU/w819-h546-no/02+Choose+No+configuration+for+Mail+Service.jpg)

選擇 Yes 以使用 Drupal 的 MySQL 預設套裝設定：
![Configure Database for Drupal](https://lh6.googleusercontent.com/-MbHCb1rNbRM/Ugxmhz4uzhI/AAAAAAAAf40/VBRDsFlGdwM/w787-h546-no/03+Configure+Database+for+Drupal.jpg)  

選擇 MySQL 做為資料庫系統：
![Choose MySQL Database System](https://lh3.googleusercontent.com/-1dlx88_ephk/Ugxmi90VasI/AAAAAAAAf5Q/f0w5NA5IIOY/w819-h546-no/04+Choose+MySQL+Database+Type.jpg)

輸入 MySQL 的 root 密碼，以提供設定的權限：(本文範例 <code>root.password</code> )
![Use root Password to Setup Drupal](https://lh3.googleusercontent.com/-FQKXdFSrGmg/Ugxmi7ZFS0I/AAAAAAAAf5U/WAA6m_39lc4/w819-h546-no/05+Use+root+Password+to+Setup+Drupal.jpg)

設定 Drupal 存取 MySQL 的密碼：
(本文使用 <code>drupal.password</code> 做為 drupal 的存取密碼)
![Set Password for Drupal](https://lh4.googleusercontent.com/-ZfyIxA_N4L4/Ugxmi6_WgDI/AAAAAAAAf5M/WbLdqYgfbnk/w819-h546-no/06+Set+Password+for+Drupal.jpg)

以上作業完成後，請安裝 phpMyAdmin：
```bash
sudo aptitude install phpmyadmin
```
選擇 Yes 以預設值安裝：
![Configure Database for phpMyAdmin](https://lh6.googleusercontent.com/-fgSb2S3M1yk/Ugxmjh8xWlI/AAAAAAAAf50/8PrmzAR2ufI/w819-h546-no/07+Configure+Database+for+phpMyAdmin.jpg)  

輸入 MySQL 的 root 密碼，以提供設定的權限：(本文範例 <code>root.password</code> )
![Use root Password to Setup phpMyAdmin](https://lh3.googleusercontent.com/-9dJk5jNetSI/UgxmkL2odsI/AAAAAAAAf5w/SpWE7to7aqc/w819-h546-no/08+Use+root+Password+to+Setup+phpMyAdmin.jpg)

設定 phpMyAdmin 存取 MySQL 的密碼：
(本文使用 <code>phpmyadmin.password</code>)
![Set Password for phpMyAdmin](https://lh6.googleusercontent.com/-po92ErgW2Y4/UgxmkJQ7QkI/AAAAAAAAf5s/NipA4TLvmgA/w819-h546-no/09+Set+Password+for+phpMyAdmin.jpg)

選擇 apache2 做為網頁伺服器：
![Choose Apache](https://lh5.googleusercontent.com/-ZVDE-nWWWog/Ugxmklcc32I/AAAAAAAAf54/9VaDdNfxkXk/w819-h546-no/10+Choose+Apache.jpg)

放置 phpMyAdmin 的頁面路徑，並移除不必要檔案：
```bash
cd /var/www/ && sudo ln -s /usr/share/phpmyadmin && 
sudo rm index.html && 
cd
```

###進行 Drupal 的安裝
請下載 Drupal，並解壓縮至 /var/www/drupal 資料夾：  
(本文使用 <code>drupal-7.23.tar.gz</code>，請自行更換為所需版本)
```bash
wget http://ftp.drupal.org/files/projects/drupal-7.23.tar.gz &&
tar zxvf drupal-7.23.tar.gz && rm drupal-7.23.tar.gz &&
sudo mv drupal-7.23/ /var/www/tab/
```
另請注意，/var/www/<code>tab</code> 中，<code>tab</code> 即本次要建立之網站代號，亦將成為 URL 路徑，請依喜好自行命名 (請使用不含空格的英文字母命名)。  
接下來，請為這個 <code>tab</code> 網站製作設定檔，並設定網站資料夾的權限：  
(一樣請注意，請自行更名所有 /var/www/<code>tab</code> 路徑中的 <code>tab</code>)
```bash
cp /var/www/tab/sites/default/default.settings.php /var/www/tab/sites/default/settings.php && 
mkdir /var/www/tab/sites/default/files && 
chmod -R 775 /var/www/tab/ && 
chmod -R 777 /var/www/tab/sites/default/files/ && 
chmod 777 /var/www/tab/sites/default/settings.php && 
sudo chown -R www-data:www-data /var/www/tab/
```


###建立網站所需之資料庫
本步驟將於 MySQL 中建立 <code>tab</code> 網站所要依存之資料庫。  
  
請以 root 身份登入 MySQL，並建立 <code>tab</code> 資料庫予 <code>tab</code> 網站使用：
```bash
mysqladmin -u root -p create tab && 
mysql -u root -p
```
過程中，會詢問 root 的密碼以取得操作權限。(本文之 root 密碼為 <code>root.password</code>)  
這裡要提醒的是，資料庫名稱不一定要與網站名一樣，但建議採用相同名稱，較易於聯想。  
  
接著，指派 <code>tab</code> 資料庫的管理者，並設定此資料庫的存取密碼：  
(本文指派 root 來管理 <code>tab</code> 資料庫，並將 <code>tab</code> 資料庫的存取密碼設定為 <code>tab.password</code>)
```mysql
GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, INDEX, ALTER, CREATE TEMPORARY TABLES, 
LOCK TABLES ON tab.* TO 'root'@'localhost' IDENTIFIED BY 'tab.password';
```
再次提醒，<code>LOCK TABLES ON tab.*</code> 中的 <code>tab</code>，即為所要指定的 <code>tab</code> 網站資料庫，請自行更名；行末 <code>IDENTIFIED BY 'tab.password';</code> 中的 <code>tab.password</code> 也請自行替換。  
套用權限設定後，登出 MySQL：
```mysql
FLUSH PRIVILEGES; & \q
```
重新啟動 Apache
```bash
sudo /etc/init.d/apache2 restart
```

###啟用網站
於瀏覽器開啟：[http://localhost/<code>tab</code>/](http://localhost/tab/)  
(以遠端登入網站伺服器者，請將 URL 中的 <code>localhost</code> 更名為該伺服器之 IP 或 Domain。)
![Choose Profile](https://lh5.googleusercontent.com/-DT3tbGbLDC4/UgxmlX95lqI/AAAAAAAAf6Q/TY7d7fgOW6g/w918-h487-no/11+Choose+Profile.jpg)
   
選擇偏好的語言：
![Choose Language](https://lh3.googleusercontent.com/-i2cdLfsEUZo/UgxmlXgbJoI/AAAAAAAAf6M/FAj_hY9iiFg/w918-h487-no/12+Choose+Language.jpg)

請輸入這個網站的資料庫名稱、資料庫管理者名稱及密碼，以連接資料庫：  
(本文的網站資料庫為 <code>tab</code>、管理者為 <code>root</code>、密碼為 <code>tab.password</code>)
![Set Connection to Web Database](https://lh3.googleusercontent.com/-F0LiFPF5ywQ/UgxmlYigh7I/AAAAAAAAf6I/YPtFA6W2kEw/w887-h546-no/13+Set+Connection+to+Web+Database.jpg)
  
設定網站管理者帳密 (並非資料庫管理者)：
(本文設定網站管理者帳號為 <code>web.admin</code>、密碼為 <code>web.password</code>、Email為 <code>web＠tab.com</code>)
![Set Site Information](https://lh5.googleusercontent.com/-Vqx6SqQVh6M/UgxmmU24B9I/AAAAAAAAf6s/qphUHeHY92A/w384-h546-no/14+Set+Site+Information.jpg)
也請順手設定一下國家、時區及更新通知。  
完成無誤時：
![Finished Info](https://lh5.googleusercontent.com/-mwg5ytSXjRc/UgxmmfnSCGI/AAAAAAAAf6k/z4B-SRMDg_Y/w918-h479-no/15+Finished+Info.jpg)
新網站，什麼都沒有，請開工吧～
![Browse the Site](https://lh3.googleusercontent.com/-mpjYLzMoj3s/UgxmmU0G29I/AAAAAAAAf6g/YlvjqSHa4a4/w816-h546-no/16+Browse+the+Site.jpg)
DONE.
<br>
<br>

補充說明
=
* **本文網站設定資訊**
  * 本機資料夾：<code>/var/www/tab/</code>
  * 本機網址：http://localhost/tab/
  * 管理者：<code>web.admin</code>
  * 密碼：<code>web.password</code>
* **本文資料庫設定
  * 管理頁面入口：http://localhost/phpmyadmin/**
  * 資料庫系統管理者 <code>root</code>，密碼 <code>root.password</code>
  * Drupal資料管理者 <code>root</code>，密碼 <code>drupal.password</code>
  * phpMyAdmin管理者 <code>root</code>，密碼 <code>phpmyadmin.password</code>
  * <code>tab</code>網站資料庫管理者 <code>root</code>，密碼 <code>tab.password</code>
* **基本資安補強**
  * 不需遠端登入資料庫時，請：<code>sudo rm /var/www/phpmyadmin</code>，反之，可：<code>cd /var/www/ && sudo ln -s /usr/share/phpmyadmin</code>

參考資源
=
* [Drupal - Community Ubuntu Documentation](https://help.ubuntu.com/community/Drupal)
* [Drupal core | drupal.org](https://drupal.org/project/drupal)
