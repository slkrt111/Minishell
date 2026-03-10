Minishell est une réplique simplifiée du shell Bash développée dans le cadre du cursus de l'École 42. Ce projet consiste à recréer un interpréteur de commandes en C, en gérant les processus, les signaux et les redirections.

Fonctionnalités implémentées:

Gestion des commandes
Affichage d'un prompt en attente d'une nouvelle commande.

Historique des commandes (utilisant la bibliothèque readline).

Exécution de commandes simples avec recherche dans le PATH (ex: ls, grep, cat).

Chemins relatifs et absolus (ex: ./minishell ou /bin/ls).

Redirections et Pipes
Pipes (|) : Le flux de sortie de chaque commande est connecté à l'entrée de la suivante.

Redirections d'entrée (<) : Lit depuis un fichier.

Redirections de sortie (>) : Écrit dans un fichier (écrase).

Redirections de sortie en ajout (>>) : Écrit dans un fichier (append).

Here-doc (<<) : Lit l'entrée jusqu'à une ligne contenant le délimiteur spécifié (ne met pas à jour l'historique).

Built-ins
Les commandes internes suivantes ont été recréées :

echo (avec l'option -n).

cd (uniquement avec un chemin relatif ou absolu).

pwd (sans options).

export (sans options).

unset (sans options).

env (sans options ni arguments).

exit (sans options).

Environnement et Signaux
Variables d'environnement : Expansion des variables $VAR et du code de retour $?.

Quotes : Gestion des simples quotes ' (empêche l'expansion) et doubles quotes " (autorise l'expansion).

Signaux :

Ctrl-C : Affiche un nouveau prompt sur une ligne vide.

Ctrl-D : Quitte le shell.

Ctrl-\ : Ne fait rien (comme dans Bash).

Installation et Utilisation
Prérequis
Le projet nécessite la bibliothèque readline. Sur Linux/Ubuntu :

Bash

sudo apt-get install libreadline-dev
Compilation
Clone le dépôt et utilise le Makefile :

Bash

git clone https://github.com/slkrt111/minishell.git
cd minishell
make
Lancement
Bash

./minishell
Architecture du projet
Parser : Analyse la ligne de commande, gère les quotes et divise les arguments en "tokens".

Expander : Remplace les variables d'environnement ($HOME, $?, etc.) par leur valeur réelle.

Executor : Gère la création des processus (fork), l'ouverture des fichiers (open), les pipes (pipe et dup2) et l'exécution des commandes (execve).

Built-ins : Fonctions internes exécutées directement par le processus parent (sauf dans certains cas de pipes).
