 Minishell
A simplified Bash-like command language interpreter.

Un interpréteur de commandes complet développé en C, axé sur la gestion des processus et la communication inter-processus.

 Sommaire
Introduction

Features

Installation

Usage

Technical Implementation

 Introduction
Ce projet consiste à recréer un shell minimaliste. L'objectif est de comprendre en profondeur le fonctionnement interne d'un terminal : la lecture d'une entrée, le parsing de tokens, l'expansion de variables, et l'exécution de processus via des appels système Unix.

[!NOTE]
Ce projet respecte strictement la partie obligatoire du sujet 42 (pas de bonus inclus).

 Features
Prompt Interactif : Affichage d'un prompt fonctionnel avec gestion de l'historique via readline.

Exécution de Commandes :

Recherche dans le PATH pour les commandes système (ls, grep, cat, etc.).

Gestion des chemins relatifs (./script.sh) et absolus (/bin/ls).

Built-ins (Commandes Internes) :

echo avec option -n.

cd, pwd, export, unset, env, exit.

Redirections & Pipes :

Redirection d'entrée < et Here-doc <<.

Redirection de sortie > (overwrite) et >> (append).

Pipes | pour connecter les flux entre plusieurs commandes.

Gestion des Signaux :

Ctrl-C : Nouveau prompt.

Ctrl-D : Quitter proprement.

Ctrl-\ : Aucun effet (conforme à Bash).

🛠️ Installation
1. Cloner le dépôt
Bash

git clone git@github.com:votre-user/minishell.git
cd minishell
2. Compilation
Le projet utilise un Makefile standard pour compiler les sources :

Bash

make
 Usage
Une fois compilé, lance l'exécutable :

Bash

./minishell
Exemple de commandes supportées :

Bash

minishell> ls -l | grep "minishell" > output.txt
minishell> echo $USER is using $SHLVL
minishell> export MY_VAR="42"
minishell> cat << END
> hello world
> END
⚙️ Technical Implementation
Le projet est divisé en quatre grandes étapes logiques :

1. Lexing & Parsing
La ligne de commande est découpée en Tokens (mots, opérateurs, redirections). Le parser s'assure que les quotes (' et ") sont fermées et correctement interprétées.

2. Expansion
Avant l'exécution, le shell parcourt les tokens pour remplacer les variables d'environnement (ex: $USER) par leur valeur réelle et traite le code de retour $? de la dernière commande.

3. Execution Engine
C'est le cœur du projet utilisant les appels système :

fork() : Création de processus fils pour chaque commande.

pipe() : Création de tunnels de communication entre les processus.

dup2() : Redirection des descripteurs de fichiers (STDIN, STDOUT).

execve() : Remplacement du processus actuel par le programme à exécuter.

4. Signal Handling
Utilisation de sigaction pour intercepter les interruptions clavier et maintenir l'intégrité du prompt sans fuite de mémoire.
