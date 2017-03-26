# Relazione per il primo progettino
Autori : Enrico Dal Ben, Haki Dine

## Parte I : Creazione di un terreno basato su heightmap
Autore : Enrico Dal Ben

### Studio e modifica del codice iniziale
Partendo dal codice iniziale fornito dal Professor Ranon nel file StartingCode-nolights.html ho creato un cubo di base con la texture inspirata ai cubi di Minecraft.
Per creare il cubo ho caricato 6 texture su un vettore e poi ho generato il materiale con cui ricoprire il cubo.
```javascript
  	var material1 = new THREE.MeshBasicMaterial( { map: new THREE.TextureLoader().load('textures/border.jpg') } );
	var material2 = new THREE.MeshBasicMaterial( { map: new THREE.TextureLoader().load('textures/border.jpg') } );
	var material3 = new THREE.MeshBasicMaterial( { map: new THREE.TextureLoader().load('textures/grass.jpg') } );
	var material4 = new THREE.MeshBasicMaterial( { map: new THREE.TextureLoader().load('textures/dirt.jpg') } );
	var material5 = new THREE.MeshBasicMaterial( { map: new THREE.TextureLoader().load('textures/border.jpg') } );
	var material6 = new THREE.MeshBasicMaterial( { map: new THREE.TextureLoader().load('textures/border.jpg') } );
  
	var materials = [material1, material2, material3, material4, material5, material6];
	var meshGrassMaterial = new THREE.MultiMaterial( materials );
```
Utilizzando il codice presente nel file StartingCode-heightmap.html ho capito come calcolare il colore di un pixel in una heightmap e come ritornare un vettore contenente i dati sulla tonalità di grigio utilizzando la funzione *getHeightData(img)*.

![Cubo iniziale](textures/cubo.jpg)

### Sviluppo
Partendo dal cubo iniziale, con texture applicata, ho creato inizialmente un ciclo che mi andava a clonare l'oggetto e poi ha spostarlo nella posizione corretta creando inizialmente un terreno piatto.
Sviluppando ulteriormente questa funzione in una funzione chiamata *CreateTerrain(width, height, data)* ho aggiunto i cubi e combinando il risultato fornito da *getHeightData(img)* sull'immagine, ho modificato la loro coordinata y per adattarla al colore del pixel : più chiaro è il pixel più in alto si troverà il cubo.

![risultato intermedio](textures/complete.jpg)

Il risultato del metodo *CreateTerrain(width, height, data)* sembra soddisfacente solo ne caso in cui la differenza di altezza da un cubo a un altro si di una unità e quindi non generino spazi vuoti.
Per risolvere questo problema ho modificato il codice di *CreateTerrain* aggiungendo un ciclo che, utilizzando le coordinate di quel punto, generi tutti i cubi fino al piano che ha y=0.
Questi cubi non sono però uguali a quelli generati sopra ma bensi sono dei cubi formati solo dalla texture *dirt.jpg*.
Per realizzare questo progetto ho preso spunto dalle texture di Minecraft poichè offre ottimi spunti per il lavoro con cubi e texture applicabili a essi.

![Risultato finale](textures/complete.jpg)

## Parte II : Movimenti di camera e creazione di un video post-procesato
Autore : Hake Dine

### Parte iniziale 
Dopo aver concluso la parte del "Create Terrain" dovevo fare un "movie" usando i movimenti di camera di three.js, inquadrando sempre il terreno creato dal mio collega. Per fare ciò, ho fatto uso degli attributi position e lookat della varibile camera, che è una Perspective Camera.
  
```javascript
  	camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 );
```
Per prima cosa, ho salvato il valore del tempo in cui iniziano le animazioni in una variabile chiamata "timestart". Questo è reso possibile dalla funzione "Date.now()". Successivamente ho creato una funzione chiamata "CameraChanges(Date.now())" in cui passo il tempo che scorre, e che ho posizionato nella funzione 
"Update()" già creata dal proffessor Ranon. In questa funzione userò condizioni "if()" nelle quali userò di nuovo la funzione "Date.now()",che mi da il tempo che sta trascorrendo, alla quale sottraggo la variabile "timestart". In questo modo  posso dettare i tempi di quando cambio i movimenti della camera

```javascript
	var timestart=Date.now();
```	
```javascript	
	function Update() {
			requestAnimationFrame( Update );
			CameraChanges(Date.now());
			stats.update();
			Render();
	}
```
```javascript	
	function CameraChanges(Date){
		if(Date-timestart<3000){
			............
			............
		}
```

### Applicare movimenti di camera

Per quanto riguarda le proprietà position delle coordinate x,y e z ho usato dei coefficenti a,b e c che usavo come misura di spostamento in positivo o in negativo. 
Quest'ultimi gli ho moltiplicati per dei numeri costanti diversi ogni volta: più grande è il numero costante, più veloce avviene la transizione della camera nella posizione voluta.  
La proprietà lookAt invece, l'ho usata ad ogni cambiamento di positione della camera in modo da dire alla camera virtuale di puntare sempre nello stesso punto, che in questo caso è l'origine.
 
```javascript
	var a=0.002,b=0.002,c=0.001;
```	
```javascript	
	camera.position.x += a*24;
	
	camera.position.y -= b*40;
	
	camera.position.z -= c*100;
	
	camera.lookAt( new THREE.Vector3(0,0,0));
```

Per i cambi totali di inquadratura ho usato sempre gli attributi position, ma ho applicato delle condizioni su una variabile chiamata shots. Questa varabile parte da valore 0, e ad
ogni cambio di inquadratura totale si incrementa di uno. Dentro alle condizioni riposiziono la camera e incremento shots. 
 
```javascript
	var shots=0;
```	
```javascript	
	if(shots==0){
		camera.position.x = -5;
		camera.position.y = 30;
		camera.position.z = 30;
		shots+=1;		
	}	
	camera.position.x += a*50;
	camera.lookAt( new THREE.Vector3(0,0,0));
```

Dopo aver finito tutti i movimenti/cambi di inquadratura da me voluti, ho usato il software di screen capturing chiamato Ice Screen Recorder per fare il video.