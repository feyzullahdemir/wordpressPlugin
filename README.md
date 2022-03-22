# wordpressPlugin
Basit Şekilde Wordpress plugin oluşturma gösterilmiştir.
index.php dosyası oluşturarak işe başlıyoruz  indexçphp dışında bir dosya oluşturacak isek wordpress bu dosyayı algılaması için define ile göstermemiz gerekir.
İndex.php içine şöyle yazıyoruz 


```PHP
<?php 

/*
	Plugin Name: İlk Eklentim
	Plugin URI: https:yoursite.com
	Description: yeni eklentim
	Version: 0.0.1
	Author: Feyzullah Demir
	Author URI: https:your.site
	License: GNU
	*/


global $wpdb;
$charset = $wpdb->get_charset_collate();
$tableName = $wpdb->prefix."formName";
$sql = "CREATE TABLE $tableName (
id INT(55) NOT NULL AUTO_INCREMENT , 
Name varchar(255) NOT NULL ,
Surname varchar(255) NOT NULL ,
UNIQUE KEY id (id) 
) $charset;";
require_once (ABSPATH. "wp-admin/includes/upgrade.php");

dbDelta($sql);

register_activation_hook(__FILE__ , 'creating_plugin_table');


//eklenecek yeri seçiyorsunuz ve eklenecek fonskiyonu
add_action("admin_menu" , "plugin");


function plugin () {
	//bura  da plugin hakkında özellikleri giriyoruz en son yazdığımız ise çalıştırması gerek fonskiyon
	
add_menu_page("PluginTitle" , "PluginName", "manage_options" , "pluginSite" , "pluginContents");

}

function pluginContents () 

{
	//Eklenti sayfaı içerğini buraya eklemeniz gerekir.
	echo "İlk Eklentimi Oluşturdum.";

	//Basit bir Form örneği yapalım

?>
<form method="POST" action="">
  <label for="fname">First name:</label><br>
  <input type="text" id="fname" name="Name"><br>
  <label for="lname">Last name:</label><br>
  <input type="text" id="Surname" name="Surname">
  <input type="submit" name="Gönder" value="gönder">
</form>

<?php
//Yukarıda Basit bir form oluşturduk burada ki 2 değeri post edip ekrana yazdıracağız.


//Yukarıda ise post ettiğimiz değerleri ekrana yazdırdık şimdi bunları veri tabanına ekleyelim ilk önce veri tabanı oluşturamamız gerekiyor bunun içinde wpdb parametersini kullanacağız.


if(isset($_POST['Gönder']))
{

global $wpdb;

$table_name = $wpdb->prefix."formName";
$wpdb->insert(
                    $table_name,
                    array(
                            'Name'=>$_POST['Name'],
                            'Surname'=>$_POST['Surname']
                            

                        ),
                    array( '%s','%s')
                 );
}
}


function footerName() {
    global $wpdb;
	//{$wpdb->prefix} kısmı database tablo eklerken wp_ ön ekini eklemeyi sağlar.
	//get_result ile veriyi çekiyoruz
    $wpPostData2=$wpdb->get_results("select * from {$wpdb->prefix}formName WHERE id = 2",OBJECT);
    //Veri tabanın da ki verimizi sitemizin footer kısmına çektik
    echo "<p align='right'>".$wpPostData2[0]->Name.$wpPostData2[0]->Surname."</p>";
     

}
add_action( 'wp_footer', 'footerName' );


```
-----
<img width="1440" alt="Screen Shot 2022-03-17 at 5 34 34 PM" src="https://user-images.githubusercontent.com/101548542/158829299-ca9a6f55-9096-4f33-8eff-6d86d6d0d798.png">

-----
<img width="1440" alt="Screen Shot 2022-03-17 at 4 42 20 PM" src="https://user-images.githubusercontent.com/101548542/158828858-08a89f8d-30b3-4f50-a53b-d336dea3a8ae.png">
------

<img width="1440" alt="Screen Shot 2022-03-17 at 4 46 14 PM" src="https://user-images.githubusercontent.com/101548542/158829071-abe6d666-4e71-419e-99f0-28e511417ce4.png">
------
<img width="1440" alt="Screen Shot 2022-03-17 at 4 59 19 PM" src="https://user-images.githubusercontent.com/101548542/158829427-3cff55d6-1919-4fc5-a0b7-939d5297ca5a.png">

------
