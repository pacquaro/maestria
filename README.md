# maestria
<?php
session_start();


if (!empty($_POST['opcion'])){
$opcion = $_POST['opcion'];
}else{
echo("NO PASO");
}

  require ("clases/loginClass.php");
      $login = new login;
	  
if ($opcion == "in"){

    
      if (!empty($_POST['usuario'])){
      $login->id = $_POST['usuario'];
      }

      if (!empty($_POST['contrasenia'])){
      $login->contrasenia = $_POST['contrasenia'];
      }

      $login->validar_id();
      if (mysql_num_rows($login->Consulta_ID) <> 0){
          $u = mysql_fetch_row($login->Consulta_ID);

          $_SESSION ['u'] = $u[0];
          $login->validar_contraseña();

           $c = mysql_fetch_row($login->Consulta_ID);
           IF ($login->contrasenia == $c[0]){

                   $_SESSION ['c'] = $c[0];
                   $_SESSION ['m'] = $_SESSION ['u'];
                   $_SESSION ['log'] = "ok";

                   $login->validar_permiso();
                   $p = mysql_fetch_row($login->Consulta_ID);

                   IF ($p[0] == '0'){
				   $_SESSION ['menu'] = "alumno";
                   $login->estadistica();
                   }
                   
				   IF ($p[0] == '1'){
				   $_SESSION ['menu'] = "administrador";
                   $login->estadistica();
				   }
				   
				   IF ($p[0] == '2'){
				   $_SESSION ['menu'] = "profesor";
				   $login->estadistica();
				   
				   
				   
                   }
           }
               else{
                   $_SESSION ['m'] = "Password incorrecto";
                       }
      }
      else{
           $_SESSION ['m'] = "Usuario Inexistente";

      }
}

if ($opcion == "out"){
 
$login->id = $_SESSION["u"];
$login->estlogout();

 $_SESSION ['log'] = "";
 
 session_destroy();

//setcookie (session_name(), "",time() -6);

}

if (isset($_SERVER["HTTP_REFERER"])){
header("location: ".$_SERVER["HTTP_REFERER"], FALSE);
exit;
}
?>
