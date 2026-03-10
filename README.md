#  Minishell

<p align="center">
  <strong>A simplified Bash-like command language interpreter.</strong><br>
  <em>Un interpréteur de commandes complet développé en C, axé sur la gestion des processus et la communication inter-processus.</em>
</p>

---

##  Sommaire
* [Introduction](#-introduction)
* [Features](#-features)
* [Installation](#-installation)
* [Usage](#-usage)
* [Technical Implementation](#-technical-implementation)

---

##  Introduction
Ce projet consiste à recréer un shell minimaliste. L'objectif est de comprendre en profondeur le fonctionnement interne d'un terminal : la lecture d'une entrée, le parsing de tokens, l'expansion de variables, et l'exécution de processus via des appels système Unix.

---

##  Features

* **Prompt Interactif** : Affichage d'un prompt fonctionnel avec gestion de l'historique via `readline`.
* **Exécution de Commandes** :
    * Recherche dans le `PATH` pour les commandes système (`ls`, `grep`, `cat`, etc.).
    * Gestion des chemins relatifs (`./script.sh`) et absolus (`/bin/ls`).
* **Built-ins (Commandes Internes)** :
    * `echo` avec l'option `-n`.
    * `cd`, `pwd`, `export`, `unset`, `env`, `exit`.
* **Redirections & Pipes** :
    * Redirection d'entrée `<` et Here-doc `<<`.
    * Redirection de sortie `>` (overwrite) et `>>` (append).
    * Pipes `|` pour connecter les flux entre plusieurs commandes.
* **Gestion des Signaux** :
    * `Ctrl-C` : Affiche un nouveau prompt.
    * `Ctrl-D` : Quitte proprement (EOF).
    * `Ctrl-\` : Aucun effet (conforme à Bash).

---

##  Installation

### 1. Cloner le dépôt
```bash
git clone git@github.com:votre-user/minishell.git
cd minishell
```
### 2. Compilation
```bash
make
```
### 2. Lancement de l'éxecutable
```bash
./minishell
```
---
##  Technical Implementation

### 1. Lexing & Parsing
La ligne de commande est découpée en Tokens (mots, opérateurs, redirections). Le parser s'assure que les quotes (' et ") sont fermées et correctement interprétées.

### 2. Expansion
Avant l'exécution, le shell remplace les variables d'environnement (ex: $USER) par leur valeur réelle et traite le code de retour $?.

### 3. Execution Engine
Utilisation intensive des appels système Unix :

fork() : Création de processus fils.

pipe() : Création de tunnels de communication.

dup2() : Redirection des descripteurs (STDIN, STDOUT).

execve() : Exécution des programmes.

### 4. Signal Handling
Utilisation de sigaction pour gérer les signaux sans interrompre le processus parent de manière inattendue.
