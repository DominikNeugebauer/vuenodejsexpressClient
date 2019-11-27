<h1>Nodejs und Express mit Vuejs/axios</h1>
<p>In diesem Dokument wird kurz Zusammengefasst, wie man eine Full Stack Webapplikation mit Nodejs Express im backend und Vuejs mit axios im Frontend Entwickeln kann. Hier habe ich als Beispiel eine Post Applikation geschrieben. Benutzer der Seite können Posts erstellen, anzeigen und löschen.</p>

<h2>Start</h2>
<p>Gestartet wird mit einem Nodejs Express Server. Ein Nodejs Projekt wird gestartet mit der Erstellung einer package.json Datei. Diese wird erstellt mit dem Konsolen Befehl npm init. Danach können alle notwendigen Pakete installiert werden. In diesem Fall die Pakete:</p>

<ul>
<li>Express (backend Framework)</li>
<li>Mongodb (Datenbank)</li>
<li>cors (Middleware)</li>
<li>body-parser (Requestbodies übersetzen)</li>
</ul>

<p>Diese können mit dem Befehl npm install "paketname" --save installiert werden. Nun wird der Server geschrieben. In diesem Beispiel ist der Server eine API, welche auf die Rute /api/posts reagiert. Hier kann man mit delete, post und get daten austauschen (Siehe app.js in server Ordner).</p>

<h2>Front End (Vuejs)</h2>
<p>Zu erst wird das Frontend getrennt vom Server erstellt. Das heißt, dass der Server die DAtein vorerst nicht an den Client sendet. Erst nach der Erstellung des Frontends wird dieses "kompiliert" und am Server eingebunden. Um mit dem Frontend zu beginnen wird ein paket benötigt. Die Vue CLI. Installation: npm install -g @vue/cli. Nach der Installation kann über die CLI ein Vue Projekt erstellt werden mit dem Befehl: vue create client. Starten kann man ein neues vue projekt mit dem Befehl: npm run serve</p>

<p>Die Grundlagen von Vuejs werden in diesem Dokument nicht erklärt. Was aber erklärt wird ist die axios Programmierung. Axios ist ein JS Paket, welches dem Programmierer ermöglicht Daten im Hintergrund auszutauschen ohne die View (html) neu laden zu müssen. In Verbindung mit Vuejs kann man sozusagen dauernd Daten empfangen und senden ohne ein einziges Mal die View neu laden zu müssen. Diese Paket muss natürlich wieder installiert werden mit dem Befehl: npm install axios (natürlich im Vue Projektordner und nicht am Server).</p>

<h2>Axios</h2>
<p>Um mit Axios Requests zu behandeln gibt es eingebaute Funktionen. Wir benötigen in diesem Beispiel nut post, get und delete Requests. Die dazugehörigen Funktionen heißen axios.get, axios.post und axios.delete. Ein Beispiel mit get (Alle posts von der Datenbank holen):</p>
<code>
async loadPosts(){
    await axios.get(this.url).then(response => {
    this.posts = response.data;
    this.isLoaded = true;
    });
}
</code>
<p>Der Erste Parameter ist die URL. Danach kann man mit .then() den response herausfiltern und Alles damit machen was man möchte. In diesem Fall speichere ich die Daten des Servers in eine Variable. Post und delete ist sehr ähnlich.</p>

```
 async createPost() {
    this.isLoaded = false;
    const textSend = this.text;
    await axios.post(this.url, {
        "text": textSend
    })
    this.posts = [];
    this.loadPosts();
},
async deletePost(id){
    this.isLoaded = false;
    await axios.delete(this.url+"/"+id);
    this.posts = [];
    this.loadPosts();
},
```

<h2>Einbindung in Server</h2>
<p>Wenn das Frontend fertiggestellt ist, kann man dieses "kompilieren" und ins backend einbinden. Zu erst muss "kompiliert" werden. Das geht mit dem Befehl npm run build. Dieser Befehl "kompiliert" das Projekt in einen neuen Ordner mit dem Namen dist. Dieser wird am Server gespeichert unter public (neu erstellen). Als nächstes muss definiert werden, dass der Server auch Datein im public Ordner verwenden darf.</p>

<code>
app.use(express.static(__dirname + "/public"));
</code>

<p>Die index.html Datei, welche erstellt wurde kann an den client gesendet werden mit der Methode res.sendFile().</p>

<code>
res.sendFile(__dirname +"/public/index.html");
</code>

<p>Nun ist das Projekt komplett. Bei Aufruf der definierten Rute wird die Datei an den Client gesendet. Der rest Bleibt gleich. !Wichtig: Die index.html Datei kann nur über einen webserver gestartet werden!</p>