# **Document de conception**

## description fonctionnelle

### informations générales

CloudMonitor est une application Android développée par Killian et Simon. Elle permet de surveiller l'état d'une plateforme Cloud directement depuis un téléphone. Ce projet se distingue par quatre fonctionnalités principales :
- **Récupération de données**
- **Utilisation actuelle des ressources**
- **Utilisation du CPU dans le temps**
- **Changement de l'adresse du serveur**

### Récupération de données

La récupération des données du serveur se fait via la méthode **fetchMetrics()**, qui utilise la classe **JsonArrayRequest** pour envoyer une requête HTTP au serveur et récupérer les données au format JSON. Une fois les données récupérées, la méthode **updateRecyclerView()** est utilisée pour afficher les résultats dans un **RecyclerView**.

### Changement de serveur

L'utilisateur peut modifier l'adresse IP du serveur à interroger en utilisant une activité dédiée. Cette interaction se fait par **startActivityForResult()** dans la **MainActivity**, et le résultat est traité dans **onActivityResult()**. Dans l'activité de modification d'IP (par exemple **IpActivity**), l'adresse IP saisie est renvoyée à l'activité principale via un **Intent Extra**.

- **Méthodes utilisées** :
  - **startActivityForResult()** (pour ouvrir l'activité de changement d'adresse IP)
  - **onActivityResult()** (pour récupérer le résultat dans l'activité principale)
  - **setResult()** (pour envoyer l'IP modifiée à l'activité principale via un **Intent**)

### Affichage des données

Les données récupérées sont affichées à l'aide d'un **RecyclerView** dans l'activité principale. Le **RecyclerView** est mis à jour par la méthode **updateRecyclerView()**, qui utilise un **Adapter** (par exemple, **MyAdapter**) pour lier les données aux éléments de la vue.

### Fonctionnalité de suivi CPU

L'utilisation du CPU dans le temps est affichée à l'aide de la bibliothèque **MPAndroidChart**. La méthode **showCpuUsageGraph()** permet d'afficher un graphique linéaire des données CPU collectées.

- **Méthodes utilisées** :
  - **showCpuUsageGraph()** (pour afficher le graphique)
  - **LineChart** (pour l'affichage du graphique)
  - **LineDataSet** et **LineData** (pour préparer les données du graphique)

### Schéma de fonctionnement

1. **MainActivity** : Permet de récupérer et afficher les métriques des ressources via un bouton. Elle inclut également un bouton pour modifier l'adresse IP du serveur et afficher l'utilisation du CPU.
2. **IpActivity** : Permet de modifier l'adresse IP du serveur à interroger. Cette activité utilise **startActivityForResult()** pour renvoyer l'IP modifiée à l'activité principale.
3. **CpuActivity** : Affiche l'utilisation du CPU au fil du temps à l'aide de graphiques linéaires.
4. **GraphActivity** : Affiche un graphique basé sur les données collectées, telles que l'utilisation des ressources.

---

L'application repose sur une architecture simple où les activités sont liées via des **Intent** pour passer des données entre elles et sur des appels HTTP pour récupérer les données du serveur. La gestion des ressources est optimisée avec l'utilisation d'un **RecyclerView** pour afficher les données dynamiques et un **Graph** pour les analyses graphiques.
