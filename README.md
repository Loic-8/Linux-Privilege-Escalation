# Escalade de Privilèges sur un Système Linux

Ce document détaille les étapes et les différentes tentatives pour obtenir un accès `root` à partir d'un shell utilisateur à bas privilèges sur une machine Metasploitable 2.

## 🎯 Objectif

Élever les privilèges du simple utilisateur `tomcat55` à `root`.

## 🔬 Environnement du Laboratoire

-   **Machine Attaquante :** Kali Linux
-   **Machine Cible :** Metasploitable 2

## 👣 Méthodologie et Tentatives

### 1. Prise d'Accès Initial
Un accès initial a été obtenu en tant qu'utilisateur `tomcat55` en exploitant une vulnérabilité de téléversement de fichier `.war` sur le gestionnaire Apache Tomcat (port 8180).

### 2. Énumération Interne
Le script **`linpeas.sh`** a été téléversé et exécuté sur la cible. L'analyse a révélé plusieurs vecteurs potentiels, notamment :
-   Une version ancienne du **Noyau Linux (2.6.24)**.
-   Une version vulnérable de **`sudo`**.
-   Le service **`distcc`** en cours d'exécution.

### 3. Tentatives d'Exploitation

-   **Tentative 1 : Exploit de Noyau (`sock_sendpage`)**
    -   **Résultat :** Échec. L'exploit a été transféré et compilé avec succès, mais son exécution n'a pas permis d'obtenir les privilèges `root`.

-   **Tentative 2 : Exploit Sudo (`-u#-1`)**
    -   **Résultat :** Échec. Le shell non-interactif initial a provoqué des erreurs de syntaxe. Même après stabilisation du shell, la version de `sudo` s'est avérée non-vulnérable à cette méthode spécifique dans ce contexte.

-   **Tentative 3 : Exploit `distcc` (Succès)**
    -   **Méthode :** L'exploit Metasploit `exploit/unix/misc/distcc_exec` a été utilisé pour cibler le service `distcc`.
    -   **Résultat :** **Succès.** L'exploit a fonctionné directement, fournissant un shell `root` immédiat.

## 🧠 Compétences acquises
-   Techniques d'énumération post-exploitation avec des scripts automatisés (LinPEAS).
-   Recherche d'exploits publics avec `searchsploit`.
-   Compilation et exécution d'exploits locaux.
-   **Pivotement :** Capacité à abandonner une piste infructueuse pour en poursuivre une autre, plus prometteuse.
-   Escalade de privilèges via une mauvaise configuration de service.
