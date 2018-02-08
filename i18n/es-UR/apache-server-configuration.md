اپاچی سرور کی تشکیل برائے ایسپو سی آر ایم (نظام برائے تعلقاتِ صارف)
یہ ہدایات سرور کی تشکیل کی راہنمائی کے ضمن میں ہیں۔ مہربانی فرما کر نوٹ کر لیجئے کہ یہاں دی گئی سب تشکیلی ترتیبات ابنٹو سرور پر بنائی گئی ہیں۔
پی ایچ پی کےلیے ضروریات
تمام ضروری لائبریریز کو انسٹال کرنے کےلیے، ان کمانڈز کو ٹرمینل پر چلائیے:
sudo apt-get update
sudo apt-get install php-mysql php-json php-gd php-mcrypt php-zip php-imap php-mbstring php-curl
sudo phpenmod mcrypt imap mbstring
sudo service apache2 restart
حل برائے مسئلہ” اے پی آئی ایرر: ایسپو سی آری ایم میں اے پی آئی کی عدم دستیابی“ :
صرف ضروری اقدامات اٹھائیے۔ ہر مرحلے کے بعد جائزہ لیجئے کہ شاید مسئلہ حل ہو گیا ہو۔ 
1.	اپاچی میں "ری رائٹ-موڈ" سپورٹ کی فعالیت
"ری رائٹ- موڈ" کی فعالیت کےلیے ٹرمینل میں ان کمانڈز کو درج کیجئے:
sudo a2enmod rewrite
sudo service apache2 restart
2.	.htaccess سپورٹ کی فعالیت
.htaccess سپورٹ کی فعالیت کےلیے سرور کی ان تشکیلی ترتیبات میں ترمیم کیجئے
 /etc/apache2/sites-available/ESPO_VIRTUAL_HOST.conf یا /etc/apache2/apache2.conf (/etc/httpd/conf/httpd.conf): 
<Directory /PATH_TO_ESPO/>
AllowOverride All
</Directory>
اس کے بعد ان کمانڈز کو ایک ٹرمینل میں چلائیے:
sudo service apache2 restart
3.	ری رائٹ بیس پاتھ کا اضافہ
اس فائل کو کھول کر درج ذیل لائن سے تبدیل کر دیجئے: /ESPOCRM_DIRECTORY/api/v1/.htaccess 
# RewriteBase /
کو
RewriteBase /REQUEST_URI/api/v1/
جہاں کہیں بھی REQUEST_URI  کسی ربط کا حصہ ہو جیسا کہ مثلاۤ
“http://example.com/espocrm/”, REQUEST_URI میں“espocrm” ہے۔
اختیار برائے ایچ ٹی ٹی پی سپورٹ کی فعالیت ( صرف FastCGI/ تیز سی جی آئی کےلیے) 
FastCGI اختیار برائے ایچ ٹی ٹی پی کو ڈیفالٹ میں سپورٹ نہیں کرتا۔ اگر آپ FastCGI استعمال کرتے ہیں تو آپ کو اپنے ورچوئل ہوسٹ یا /etc/apache2/apache2.conf (httpd.conf) میں اسے فعال کرنے کےلیے مندرجہ ذیل کوڈ درج کرنا ہوگا:
Fcgid ماڈیول کےلیے: 

<IfModule mod_fcgid.c>
  FcgidPassHeader Authorization
  FcgidPassHeader Proxy-Authorization
  FcgidPassHeader HTTP_AUTHORIZATION  
</IfModule>
FastCgi ماڈیول کےلیے:
<IfModule mod_fastcgi.c>
   FastCgiConfig -pass-header Authorization \
                        -pass-header Proxy-Authorization \
                        -pass-header HTTP_AUTHORIZATION  
</IfModule>
یہ جانچنے کےلیے کہ آپ فی الوقت کون سا ماڈیول آپ کے زیرِ استعمال ہے، اس کمانڈ کو چلا کو چلا کر ماڈیول تلاش کیجئے:
apache2ctl -M

