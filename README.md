# **Document de conception**

## Description fonctionnelle

### Informations générales

CloudMonitor est une application Android développée par Killian et Simon. Elle permet de surveiller l'état d'une plateforme Cloud directement depuis un téléphone mobile. Ce projet se distingue par quatre fonctionnalités principales :
- **Récupération de données**
- **Utilisation actuelle des ressources**
- **Utilisation du CPU dans le temps**
- **Changement de l'adresse du serveur**

### Récupération des données

Pour récupérer les données, l'application envoie une requête HTTP à l'API du serveur Cloud. Cela se fait à l'aide de méthodes comme **fetchMetrics()**, qui utilise une **Request** (par exemple **JsonArrayRequest** ou **StringRequest**) pour envoyer la requête et récupérer les réponses au format JSON.

Ensuite, les données sont traitées dans une **callback method** (comme **onResponse()**) où l'application extrait les informations nécessaires. Les résultats sont ensuite affichés dans un **RecyclerView** pour permettre à l'utilisateur de visualiser les informations.

- **Méthodes et classes utilisées** :
  - **fetchMetrics()** : Envoie la requête HTTP pour récupérer les métriques du serveur.
  - **JsonArrayRequest** : Utilisé pour envoyer une requête et recevoir un tableau JSON.
  - **onResponse()** : Traitement de la réponse JSON et mise à jour de l'UI.
  - **RecyclerView** : Affichage des données dans une liste dynamique.
  - **RecyclerView.Adapter** : Utilisé pour lier les données au **RecyclerView**.

Les métriques avec une base de temps sont enregistrées sur le téléphone grâce à un **FileWriter**. Lorsqu'une métrique spécifique est récupérée (par exemple, l'utilisation du CPU), l'application associe une donnée temporelle (**timestamp**) à cette métrique. Ensuite, l'application lit le fichier stocké sur le téléphone avant d'envoyer les données de **MainActivity** vers **CpuActivity** via un **Intent** avec un **ArrayList** en extra. 

Enfin, dans **CpuActivity**, les **ArrayLists** sont transformés en **Entry** pour être affichés sous forme de graphique dans un **LineChart**.

### Changement de serveur

L'utilisateur peut modifier l'adresse IP du serveur à interroger. Cette fonctionnalité est implémentée via une **Activity** spécifique, comme **IpActivity**, qui permet à l'utilisateur de saisir une nouvelle adresse IP dans un **EditText**. Une fois l'adresse IP saisie, l'**Intent** renvoie cette nouvelle IP à l'activité principale (par exemple, **MainActivity**) grâce à **setResult()** et **startActivityForResult()**.

- **Méthodes et classes utilisées** :
  - **startActivityForResult()** : Permet d'ouvrir une autre activité pour modifier l'IP du serveur.
  - **setResult()** : Renvoie l'IP modifiée à l'activité principale.
  - **onActivityResult()** : Récupère la nouvelle IP après que l'utilisateur l'ait modifiée.
  - **Intent** : Sert à passer des données (ici, l'IP du serveur) entre les activités.

### Suivi de l'utilisation CPU

L'utilisation du CPU au fil du temps est affichée sous forme de graphique. Pour cela, l'application utilise une bibliothèque tierce, telle que **MPAndroidChart**, pour afficher un graphique linéaire. Les données sont envoyées à un **LineChart**, où chaque point représente l'utilisation du CPU à un moment donné.

- **Méthodes et classes utilisées** :
  - **LineChart** : Affichage du graphique linéaire pour suivre l'utilisation du CPU.
  - **LineDataSet** : Permet de définir les données du graphique (p. ex., l'utilisation du CPU).
  - **MPAndroidChart** : Bibliothèque tierce pour la gestion des graphiques.

---

## **Schéma de fonctionnement**

### MainActivity
- Permet de récupérer et afficher les données des ressources via un bouton.
- Utilise un **RecyclerView** pour afficher dynamiquement les métriques.
- Permet à l'utilisateur de modifier l'adresse IP du serveur via **startActivityForResult()**.

### IpActivity
- Permet à l'utilisateur de saisir et modifier l'adresse IP du serveur.
- Renvoie la nouvelle IP à **MainActivity** via **setResult()**.

### CpuActivity
- Affiche l'utilisation du CPU sous forme de graphique linéaire avec **MPAndroidChart**.

### GraphActivity
- Affiche d'autres graphiques ou métriques détaillées si nécessaire.

L'application repose sur une architecture simple, avec l'utilisation d'**Intents** pour passer des données entre les activités et des appels HTTP pour récupérer les données depuis le serveur. Le **RecyclerView** et **MPAndroidChart** sont utilisés pour une gestion optimale des données et de l'affichage graphique.

---

## **Documentation des Classes et Méthodes**

### IpActivity

#### Attributs
- `String url` → Stocke l'URL du serveur.
- `EditText ip` → Champ de saisie permettant à l'utilisateur d'entrer une nouvelle IP.
- `Button validation` → Bouton permettant de valider l'IP saisie.

#### Méthodes importantes
- **onClick(View v)**  
  - **Paramètre** : `View v` (l'élément qui a été cliqué).
  - **Description** :  
    - Crée un nouvel **Intent** pour renvoyer la nouvelle IP.  
    - `putExtra("newURL", ip.getText().toString())` → Ajoute la nouvelle IP à l'intent.  
    - `setResult(RESULT_OK, retour)` → Retourne la nouvelle IP à l'activité principale.  
    - `finish()` → Ferme **IpActivity**.

---

### CpuActivity

#### Attributs
- `LineChart lineChart` → Graphique pour afficher l'évolution du CPU.
- `ArrayList<Entry> entryList` → Liste des entrées du graphique linéaire.
- `ArrayList<String> legDates` → Liste des dates associées aux points du graphique.
- `ArrayList<Metric> graphMetrics` → Liste des métriques du CPU récupérées.
- `Gson gson` → Objet pour manipuler le JSON.

#### Méthodes importantes
- **setupLineChart()**  
  - Configure **LineChart** avec **LineDataSet** contenant les points du CPU.
  - `setColor(Color.BLUE)` → Définit la couleur des lignes du graphique.
  - `setValueTextSize(12f)` → Ajuste la taille du texte des valeurs.
  - `setDrawCircles(true)` → Active l'affichage des points.

---

## **Schéma fonctionnel**

L'application repose sur une architecture simple, avec l'utilisation d'**Intents** pour passer des données entre les activités et des appels HTTP pour récupérer les données depuis le serveur. Vous trouverez ci-dessous le schéma fonctionnel avec les liens entre les différentes activités et un aperçu de l'application.

---

## **CpuActivity**  

### **Attributs**  
- `LineChart lineChart` → Graphique pour afficher l'évolution du CPU.  
- `ArrayList<Entry> entryList` → Liste des entrées du graphique linéaire.  
- `ArrayList<String> legDates` → Liste des dates associées aux points du graphique.  
- `ArrayList<Metric> graphMetrics` → Liste des métriques du CPU récupérées.  
- `Gson gson` → Objet pour manipuler le JSON.  

### **Méthodes et instanciations**  
- `getIntent().getParcelableArrayListExtra("donne")` → Récupère la liste des métriques CPU.  
- `new ArrayList<>()` → Instancie une liste vide pour stocker les métriques.  
- `Toast.makeText(this, "ouIIIIIIi", Toast.LENGTH_SHORT).show();` → Affiche un message temporaire.

