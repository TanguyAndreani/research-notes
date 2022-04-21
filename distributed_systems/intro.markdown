# Systèmes distribués

Je vais me baser sur quatre ressources tout au long de mes recherches:

- Distributed Systems, 3rd Edition, par Maarten van Steen et Andrew S. Tanenbaum 
- [6.824](https://pdos.csail.mit.edu/6.824/schedule.html) (donnée par Robert Morris)
- Martin Kleppmann's Designing Data-Intensive Applications
- https://dsrg.pdos.csail.mit.edu/papers/

(Les deux livres sont trouvables facilement en PDF sur libgen.)

## État du document

J'ai vu dans les très grandes lignes les objectifs des systèmes distribués en terme de résolution
de problèmes liés à la technique, maintenant ça serait bien d'avoir une liste catégorisée de cas
pratiques avec quelques détails (p2p, blockchain, analyse de données en clusters, etc.)

Pendant que je vois MapReduce il faudrait avancer sur les autres sujets.

## L'idée de systèmes distribués

Morris:

>a group of computers cooperating to provide a service.

Steen-Tanenbaum:

>a collection of autonomous computing elements that appears to its users as a single coherent system.

La seconde définition insiste bien sur le côté autonome de chaque noeud du réseau. Par exemple, un noeud donné
pourrait partir ou arriver dans le système à n'importe quel moment.

Ces définitions étant très génériques, beaucoup de choses rentrent dans le cadre des systèmes distribués:

- des algorithmes d'analyse de données à grande échelle
- les réseaux p2p
- la blockchain

Morris se concentre plus sur les systèmes distribués en tant que *infrastructure services*. Stockage des
grosses plateformes, MapReduce et réseaux P2P. Il traite bien des sujets comme la blockchain, le big data
avec Spark. Beaucoup d'études de cas.

Steen-Tanenbaum détaille plutôt tous les concepts de base et les illustre avec des exemples.

## Problématiques des systèmes distribués
  
- Augmenter les capacités de traitement grâce au parallélisme.
- Créer des systèmes résilient par la replication (des données? des entités qui traitent l'information? des deux?)
- Faire correspondre l'infrastructure à l'éparpillement des capteurs physiques.
- Répondre aux enjeux de sécurité par l'isolation (des transactions?)

## Sujets à fort enjeu

- Résilience (fault tolerance): cacher les échec de l'application utilisateur.
- Consistence (consistency): Get(k) doit me renvoyer la valeur de mon Put(k, v) le plus récent. Les serveurs "répliques" sont durs à maintenir.
- Performance: plus la charge est élevée et plus il est difficile de scale. Les bottlenecks, un mauvais équilibre de la charge des serveurs, l'initialisation et les interactions entre serveurs qui ne sont pas accélérées.
- Compromis (tradeoffs): les trois points précédents ne sont pas incompatibles, mais difficiles à faire coexister. La consistence est souvent moins prioritaire.

## Quelques aspects pratiques à voir

- RPC (Remote Procedure Calls)
- Le multithreading
- Concurrency control (ie with [locks](https://databasemanagement.fandom.com/wiki/Concurrency_Control))

