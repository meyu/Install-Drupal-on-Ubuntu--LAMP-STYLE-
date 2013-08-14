所為何來
=
以 Drupal 搭配 Ubuntu、Apache、MySQL、PHP 來架設網站 (即 LAMP)。  
本文使用 Ubuntu 的 Drupal-LAMP 套裝指令，能簡單又快速地完成架設；但由於皆為預設值安裝，安全性及效能上較難盡人意，欲架設網站長期經營者，不推薦此方法。

使用環境
=
作業系統：Ubuntu 12.04 (Server & Desktop)
網頁伺服器：Apache 2
資料庫系統：MySQL 5.5
指令碼語言：PHP 5
資料庫管理介面：phpMyAdmin 4
內容管理系統：Drupal 7.23

安裝方式
=
###進行 LAMP 架構的安裝及設定
於終端機中，輸入以下指令來更新 Ubuntu 並安裝 Drupal 所需之伺服器元件：
```bash
sudo apt-get update && sudo apt-get upgrade &&
sudo apt-get install drupal7
```
安裝過程中，請設定 MySQL 的 root (最高權限管理者) 的密碼 (本文使用 <code>root.password</code> 做為密碼)：
  
設定電子郵件伺服器 (請依實際情況自行設定；本文不進行設定)：
  
選擇 Yes 以使用 Drupal 的 MySQL 預設套裝設定：
  
以上作業完成後，請安裝 phpMyAdmin：
```bash
sudo apt-get install phpmyadmin
```
設定 phpMyAdmin 的 MySQL 使用權，選擇 Yes 以使用預設值：
  
選擇 apache2 做為網頁伺服器：

  
###進行 Drupal 的安裝
請下載 Drupal，並解壓縮至 /var/www/drupal 資料夾 (本文使用<code>drupal-7.23.tar.gz</code>，請自行更換為所需版本)：
```bash
wget http://ftp.drupal.org/files/projects/drupal-7.23.tar.gz &&
tar zxvf drupal-7.23.tar.gz && rm drupal-7.23.tar.gz &&
sudo mv drupal-7.23/ /var/www/tab/
```
另請注意，/var/www/<code>tab</code> 中，<code>tab</code> 即本次要建立之網站代號，亦將成為 URL 路徑，請依喜好自行命名 (請使用不含空格的英文字母命名)。  
接下來，請為這個 <code>tab</code> 網站製作設定檔，並設定網站資料夾的權限：
```bash
cp /var/www/tab/sites/default/default.settings.php /var/www/tab/sites/default/settings.php
mkdir /var/www/tab/sites/default/files
chmod -R 775 /var/www/tab/
chmod -R 777 /var/www/tab/sites/default/files/
chmod 777 /var/www/tab/sites/default/settings.php
sudo chown -R www-data:www-data /var/www/tab/
```
一樣請注意，所有 /var/www/<code>tab</code> 路徑中的 <code>tab</code>，請自行更名。


###建立網站所需之資料庫
本步驟將於 MySQL 中建立 <code>tab</code> 網站所要依存之資料庫。  
  
請以 root 身份登入 MySQL，並建立 <code>tab</code> 資料庫予 <code>tab</code> 網站使用：
(本文之 root 密碼為 <code>root.password</code>)
```bash
mysqladmin -u root -p create tab && 
mysql -u root -p
```
這裡要提醒的是，資料庫名稱不一定要與網站名一樣 (但建議採用相同名稱，較易於聯想)。 
指派 root 做為 <code>tab</code> 資料庫的管理者，並設定此資料庫的存取密碼：  
```mysql
GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, INDEX, ALTER, CREATE TEMPORARY TABLES, LOCK TABLES ON tab.* TO 'root'@'localhost' IDENTIFIED BY 'tab.password';
```
亦可新增其它管理者，做為 <code>tab</code> 資料庫管理者，以區別管理身份及控制權限。本文直接採用 root 做為管理者，以簡化概念。  
再次提醒，LOCK TABLES ON <code>tab</code>.* 中的 <code>tab</code> 為所要指定的網站資料庫，請自行更名。  
另，行末 IDENTIFIED BY '<code>tab.password</code>'; 中的 <code>tab.password</code> 為本文示範用，也請自行替換。  
套用權限設定後，登出 MySQL：
```mysql
FLUSH PRIVILEGES;
```
```mysql
\q
```
重新啟動 Apache
```bash
sudo /etc/init.d/apache2 restart
```

###啟用網站
於瀏覽器開啟：[http://localhost/<code>tab</code>/](http://localhost/tab/)
繼續提醒：請 <code>tab</code> 自行更名。  
另，如是以遠端登入網站伺服器者，請將 URL 中的 <code>localhost</> 更名為該伺服器之 ip 或 URL。   
選擇偏好的語言：

輸入網站的資料庫名稱、資料庫管理者名稱及密碼：  
(本文的網站資料庫為 <code>tab</code>、管理者為 <code>root</code>、密碼為 <code>tab.password</code>)

注意，以上為網站的資料庫設定。  
以下則進行網站後端管理者 (內容管理者，即，管網頁的人) 的設定：
(本文將設定的網站管理者為 <code>web.admin</code>、密碼為 <code>web.password</code>、Email為 <code>web＠tab.com</code>)

順手設定一下國家、時區及更新通知。

DONE.
<br>
<br>

補充說明
=
* 網站資料夾位置：/var/www/
* 資料庫管理頁面：http://localhost/phpmyadmin/
* 插圖後補。

參考資源
=
* [Drupal - Community Ubuntu Documentation](https://help.ubuntu.com/community/Drupal)
* [Drupal core | drupal.org](https://drupal.org/project/drupal)
