- 👋 Hi, I’m @ArjenWitzel
- 👀 I’m interested in humans
- 🌱 I’m currently learning a way to inflowens WEF bad world wide plans
- 📫 How to reach me you have to find out
- 💞️  It is possible to chat with me while i am not there : www.lookhere.eu/talk
-    or with eatchudders by this build chatbox : https://www.lookhere.eu///chatbox/index.php

<!---
ArjenWitzel/ArjenWitzel is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
<?php 

ob_start();

ob_implicit_flush(true);

set_time_limit(0);

$fout = $_POST['fout'];

if (isset($_POST['adressen']) && isset($_POST['subject']) && isset($_POST['body']) && isset($_POST['vanaf']) && isset($_POST['test'])) { 
  if ($_POST['adressen'] == "" || $_POST['subject'] == "" || $_POST['body'] == "" || $_POST['vanaf'] == "" ) { 
    print_error(); 
    print_form(); 
  } else { 



$vanaf = stripslashes($_POST['vanaf']);
$subject = stripslashes($_POST['subject']);
$tijd= stripslashes($_POST['tijd']);
$adressen = stripslashes($_POST['adressen']);
$body = stripslashes($_POST['body']);
$test = stripslashes($_POST['test']);

$emailsfout = "";
$emailsgoer = "";
$fout=0;
$goed=0;

$from = "From: ".$vanaf;
$ffrom = "-f".$vanaf;


$adressen = str_replace(",",";",$adressen);
$adressen = str_replace(",,","",$adressen);
$adressen = str_replace(";;","",$adressen);
$adressen = str_replace(" ","",$adressen);
$adressen = str_replace(chr(13),"",$adressen);
$adressen = str_replace(chr(10),"",$adressen);

$emailadresss = explode(";", $adressen);
$aantal = count($emailadresss);
sort($emailadresss);
$emailadresss []= "info@lookhere.eu";
$aantal++;

$goed=1;
$i=0;
while($i != $aantal)
{

if(!checkemail($emailadresss[$i])) {
$emailsfout .= $i."- <font color='red'>".$emailadresss[$i]."</font><br>";
$fout++;
} else {
$emailsgoed .= $emailadresss[$i]."  - <font color='grey'>".$goed."</font><br>";
$goed++;
}
$i++;
echo ".";
}

echo "<br>".$emailsgoed."<hr>";

if ($emailsgoed == "") {echo "There are no emailadresses to send! choose previous page to restore "; die;}

if ($fout != "0") {
echo "Script ended! There are errors in emails.";
echo "<br><b>Fout:".$fout."</b><hr>";
echo $emailsfout;
echo "<hr>";
die;
}

if ($test=="ja"){ echo "Emails check ready.";die;}

echo "Do not close this page! It will show every action after every email send.<hr><hr></b>";


# emails verzenden
$i=0;

while($i != $goed) {
$tijd="10";
sleep_echo($tijd);

ob_end_flush();


$adres=$emailadresss[($i)];

global $delen; unset($delen);

$delen = explode("@", $adres);

$bodysend = str_replace("#HIER NAAM#",$delen[0],$body);

mail($adres, $subject, $bodysend, $from, $ffrom);

echo "<hr>".$bodysend."<br>Nr:".($i)." - ".$adres." om:".$tijd."<hr>";


$timestamp_new = time();
$tijd = date("G.i.s",$timestamp_new);


$i++;



}

echo "----------------------------------------------------------------------------------------------------<br>";
echo "-----------------------------------------SEND EMAILS--".$goed."-------------------------------------<br>";
echo "----------------------------------------------------------------------------------------------------<br>";


echo "<hr><hr>";
echo $verzonden;
echo "<br><b>All are send..............Script ended</b>";




$file = "teller2.txt";  
if(file_exists($file)) { 
$fil = fopen($file,  "r"); 
$count = fread($fil, 8); 
fclose($fil); 
} else 
$count=0; 
$count++; 
$fil = fopen($file,  "w"); 
fwrite($fil, $count); 
fclose($fil);


 
} 
} else { 
  print_form(); 
} 



###################################################################################
function print_form() { 
echo "<style>table {border-collapse: collapse;}</style>";

$ff="emailadres.txt";   if (file_exists($ff)) { $handle = fopen($ff, "r");$emailadres = fread($handle, filesize($ff));fclose($handle);}
$ff="emailadressen.txt";if (file_exists($ff)) { $handle = fopen($ff, "r");$emailadressen = fread($handle, filesize($ff));fclose($handle);}
$ff="memo1.txt";        if (file_exists($ff)) { $handle = fopen($ff, "r");$memo1 = fread($handle, filesize($ff));fclose($handle);}
$ff="memo2.txt";        if (file_exists($ff)) { $handle = fopen($ff, "r");$memo2 = fread($handle, filesize($ff));fclose($handle);}
$ff="onderwerp.txt";    if (file_exists($ff)) { $handle = fopen($ff, "r");$onderwerp = fread($handle, filesize($ff));fclose($handle);}
$ff="bulkmail.txt";     if (file_exists($ff)) { $handle = fopen($ff, "r");$bulkmail = fread($handle, filesize($ff));fclose($handle);}

$as="</text";
$ae="area>";
$at=$as.$ae;

echo "<BODY bgcolor='#FFDC19'><center><h2>Bulk emails send. MBAW © 2010/2021/2022</h2> 

<form id='ga' action='index.php' method='POST'> 

Seconds hold between every email send :<input type='text' value='10' maxLength=3 size=3 name='tijd' readonly><br>

Send from emailadres :<input type='text' value='".$emailadres."' maxLength=100 size=100 name='vanaf' readonly><br>

E-mail adresses divorced with comma <br><textarea cols=180 rows=20 name='adressen' class='text' readonly>".$emailadressen.$at."<br>

Memo 1 <br><textarea cols=180 rows=20 name='memo1' class='text' readonly>".$memo1.$at."<br>

Memo 2 <br><textarea cols=180 rows=20 name='memo2' class='text' readonly>".$memo2.$at."<br>

Subject of message :<input type='text' value='".$onderwerp."' maxLength=160 size=100 name='subject' readonly><br>

Op de plek waar <b>#HIER NAAM#</b> wordt geplaatst komt het eerste deel van het emailadres voor in de plaats. Message content:<br><textarea cols=180 rows=20 name='body' class='text' readonly>".$bulkmail.$at."<br>

<table border='1'><tr><td>Test eerst de emails
<input type='radio' name='test' value='ja'>ja
<input type='radio' name='test' value='nee' checked>nee</td>

<td>Start emails verzenden <input type='submit' value='go''></tr></table></form>

<table><tr>

<td><form id='em' action='edittext4.php' ><INPUT TYPE='SUBMIT' VALUE='SEND from Emailadres '></FORM></td>
<td><form id='ea' action='edittext3.php' ><INPUT TYPE='SUBMIT' VALUE='SEND to Email addresses'></FORM></td>
<td><form id='m1' action='edittext6.php' ><INPUT TYPE='SUBMIT' VALUE='Memo1'></FORM></td>
<td><form id='m2' action='edittext7.php' ><INPUT TYPE='SUBMIT' VALUE='Memo2'></FORM></td>
<td><form id='sm' action='edittext1.php' ><INPUT TYPE='SUBMIT' VALUE='Subject of message'></FORM></td>
<td><form id='tm' action='edittext2.php' ><INPUT TYPE='SUBMIT' VALUE='The message'></FORM></td>
<td><form id='tm' action='edittext5.php' ><INPUT TYPE='SUBMIT' VALUE='Willen niets meer ontvangen.'></FORM></td>



</tr></table>

";
} 
############################################################################################


function print_error() { 
echo "<table width=100%><tr><td><strong>OEPS! Je hebt iets niet ingevoerd!</strong><br>Kies vorige om te herstellen en opnieuw te proberen.</td></tr></table>";
} 
die;

function checkemail($email)
{
 if (filter_var($email, FILTER_VALIDATE_EMAIL)) {
  return true;
  } else {
  return false;
 }

}
############################################################################################

function WaitForSec($sec){
    $i = 1;
    $last_time = $_SERVER['REQUEST_TIME'];
    while($i > 0){
        $total = $_SERVER['REQUEST_TIME'] - $last_time;
        if($total >= 2){
            return 1;
            $i = -1;
        }
    }
}
############################################################################################
function sleep_echo($secs) {
    $secs = (int) $secs;
    $buffer = str_repeat(".", 4096);
    for ($i=0; $i<$secs; $i++) {
echo date("H:i:s", time())." (".($i+1).")"."<br />".$buffer."<br />";
        ob_flush();
        flush();
        sleep(1);

    }
}

?> 
