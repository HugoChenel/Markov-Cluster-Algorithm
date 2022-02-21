# Markov-Cluster-Algorithm
MCL : fast and scalable unsupervised cluster algorithm for graphs

Le projet consiste à implémenter en R l’algorithme MCL de partitionnement de graphe (clustering) à partir d’un graphe et d’un paramètre nommé inflate factor (IF). L’algorithme découpe le graphe en clusters.

## Introduction

Un graphe est un objet mathématique composé de sommets (vertex) reliés par des arètes (edge). Une des représentations informatiques consiste en une matrice carrée nommée matrice d’adjacence (adjacency matrix). Une cellule de cette matrice contient le poids de l’arète reliant deux sommets ou zéro en absence de lien.
On distingue les graphes orientés (directed graph) des graphes non orientés (undirected graph). Dans un graphe orienté, les arètes sont appelées arcs et ont une direction (de A vers B). La matrice d’adjacence n’est donc plus obligatoirement symétrique et un choix arbitraire est fait concernant son interprétation : par exemple, les sommets sources correspondent aux lignes et les sommets destinations correspondent aux colonnes.

## Principes et algorithme de MCL

Une bonne méthode de clustering maximise:
- la similarité des objets appartenant à un même cluster (cohésion)
- la dissemblance entre objets appartenant à des clusters différents (séparation)

En ce qui concerne le partitionnement de graphes, une bonne méthode vise à ce que les clusters obtenus:
- aient un maximum d’arètes ou d’arcs reliant les sommets d’un même cluster
- aient un minimum d’arètes ou d’arcs reliant les sommets de clusters différents

Pour identifier les clusters, MCL se base sur la remarque suivante : si on place un marcheur aléatoire sur un sommet d’un cluster, il a plus de chances (en se promenant aléatoirement d’un sommet à un autre) de rester dans le cluster (plus grande densité de liens) que d’en sortir.

Le procédé s’appuie donc sur le calcul des probabilités de marches aléatoires (probabilité de passer par un sommet et probabilité d’emprunter un arc) dans un graphe donné. Le calcul s’effectue sur des matrices de Markov qui représentent les probabilités de transition d’un sommet à un autre ; il s’agit donc d’une matrice d’adjacence dont la somme des lignes vaut 1 (la somme des probabilités de sortie d’un sommet vaut 1). Ces matrices sont également appelées matrices stochastiques ou encore matrices de transition.

L’algorithme MCL simule des marches aléatoires dans un graphe en alternant deux oprations appelées expansion et inflation. L’expansion correspond au calcul de marches aléatoires de grandes longueurs et coincide à élever une matrice stochastique à une certaine puissance. Concrètement, l’expansion consiste à multiplier la matrice avec elle-même avec le produit matriciel. L’inflation consiste à exagérer les probabilités de marches au sein d’un même cluster et à atténuer les probabilités de marches inter-clusters. En pratique, l’inflation consiste à élever chaque cellule de la matrice à une certaine puissance (inflate factor) puis à normaliser les valeurs obtenues afin que la matrice soit stochastique.

Au final, à force de répéter les opérations d’expansion et d’inflation sur la matrice de transition (et donc sur le graphe), celui-ci est décomposé en différentes composantes (composantes connexes) déconnectées les unes des autres et qui correspondent aux clusters. En d’autres termes, l’algorithme converge et la matrice n’évolue plus.
