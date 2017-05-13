# Galeria de Imaganes Usando Albumes de Facebook
## Galeria de imagenes usando el API de los albumes de Facebook, en PHP y JS para abrir en un lightbox cada imagen.

DEMO: [http://decoprin.com/](http://decoprin.com/)

### Un poco de cuento:
Este script nació como una solución, al problema: "como hacer para que un cliente cargue las fotos de sus proyectos o lo que sea, sin tener que instalarle un CMS o hacer un sistemita en PHP+MySQL o usar otros servicios..." como flickr, google+, photobucket, etc etc... que son servicios donde se pueden crear galeria de fotos y en muchas ocasiones permiten insertar esas imagenes en portales propios colocando un par de lineas de codigo. En fin.. a pesar de que si hay N cantidad de opciones para el tema, se me ocurre usar facebook que tiene el asunto de los albumes pero a su vez tiene un API muy sencilla de usar y con buena documentación, ademas de el valor agregado: la mayoria sabe usar facebook, o sino se pudiera aprender a usar como subir fotos a facebook... esto en resumen.. ahora veamos lo que necesitamos:

### Requerimientos:
Esta demas decir los requerimientos basicos para correr PHP.
Lo que se necesita colocar para este caso es: [jquery.magnific-popup.min.js](https://cdnjs.com/libraries/magnific-popup.js/) esto para crear el lightbox donde se abriran las imagenes cargadas.

sin mas preambulo veamos los codigos:

```php
<?php
$contents = file_get_contents('https://graph.facebook.com/1698000627183449?fields=albums&access_token=1447616038876416|6eb3ba74b0511f1318257cd7cbe15e9f');
$albums = json_decode($contents,true);
$albums1 = $albums['data'];

function get_cover_photo($string){
	$url_cover_photo = 'https://graph.facebook.com/'.$string.'?fields=images&access_token=1447616038876416|6eb3ba74b0511f1318257cd7cbe15e9f';
	$photo_cover = file_get_contents($url_cover_photo);
	$get_photo_cover = json_decode($photo_cover,true);
		foreach ($get_photo_cover['images'] as $ky) {
		//if($ky['width'] == '330'){
			$src_cover = $ky['source'];
		//}
	}
	echo "<img style='margin:0 auto' class='load_photos_album img-responsive' src='".$src_cover."'>";	
	return false;
}
function get_photos($string){
$url = "https://graph.facebook.com/".$string."/photos?limit=30&fields=images&access_token=1447616038876416|6eb3ba74b0511f1318257cd7cbe15e9f";
$photos = file_get_contents($url);
$get_photos = json_decode($photos,true);
echo '<div class="get_photos">';
	foreach ($get_photos as $image) {
		foreach($image as $source => $src) {
		$photoID = $src['id'];
			if(!empty($photoID)){
				//echo '<a class="'.$string.'" href="https://graph.facebook.com/'.$photoID.'/picture">'.$photoID.'</a>';
				echo '<a class="'.$string.'" href="https://graph.facebook.com/'.$photoID.'/picture"></a>';				
			}
		}
	}
echo '</div>';
return false;
}

foreach ($albums as $key => $content) {

if (isset($content['data'])) {
    $dc = $content['data'];
	
foreach ($dc as $k => $c) {

	if($c['type'] == 'normal'){
	
	echo '<div class="col-sm-6 form-group"><div class="item-projecto">';
	echo '<div class="img-projecto">';
	
	echo '<a href="javascript:value(0);" album="'.$c["id"].'" title="Ver Galeria: '.$c["name"].'" class="load_photos_album btn btn-default">';
	
	get_cover_photo($c['cover_photo']);
	
	echo '</a>';
	
	echo '</div>';
	//echo '<h1>'.$c["name"].'</h1>';
	//echo '<p title="'.$c["description"].'">'.substr($c["description"],0,50).'...</p>';
	//echo '<a href="javascript:value(0);" album="'.$c["id"].'" title="Doble Click: '.$c["name"].'" class="load_photos_album btn btn-default">Ver Galería</a>';
	get_photos($c["id"]);
	echo '</div></div>';
	}
}	 
}
}
?> 
```

