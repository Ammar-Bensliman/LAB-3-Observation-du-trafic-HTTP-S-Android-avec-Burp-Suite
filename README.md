# LAB-3-Observation-du-trafic-HTTP-S-Android-avec-Burp-Suite
Étape 1 — Préparer Burp Suite (projet et mode Proxy)
<img width="941" height="995" alt="image" src="https://github.com/user-attachments/assets/52adffab-3661-4ef2-94cf-68e36e289adc" />
<img width="949" height="1002" alt="image" src="https://github.com/user-attachments/assets/5e75dd7a-73fe-46b2-96fe-cb80cfa22809" />
<img width="1919" height="933" alt="image" src="https://github.com/user-attachments/assets/b8a7788f-c270-4b52-92b2-350f158d1a16" />


Étape 2 — Vérifier le Proxy Listener (adresse et port)
<img width="1919" height="1000" alt="image" src="https://github.com/user-attachments/assets/2db4c02f-3d4b-468f-b966-cb4bb7203401" />

<img width="1263" height="223" alt="image" src="https://github.com/user-attachments/assets/9b2758a9-2b7d-4fd3-980e-d4d6bb7e94db" />

<img width="305" height="616" alt="image" src="https://github.com/user-attachments/assets/89659f6b-6974-4a3b-a77e-7667543d0a51" />

<img width="1919" height="495" alt="image" src="https://github.com/user-attachments/assets/d9f2df72-7c47-4c70-ae87-f1c7c7a891e8" />

<img width="817" height="559" alt="image" src="https://github.com/user-attachments/assets/59d8197f-3c5c-485f-bf16-af3a1efc4f57" />
<img width="482" height="1027" alt="image" src="https://github.com/user-attachments/assets/20906fcc-9050-4b99-9b74-ee8c62331c03" />
<img width="1919" height="582" alt="image" src="https://github.com/user-attachments/assets/828ac13b-07d7-4ebc-a850-26d044b9e614" />

Étape 9 — Produire un mini-rapport (preuve + contexte)

Étape 1 — Préparer Burp Suite (projet et mode Proxy)
Actions à réaliser :

Lancer Burp Suite.

Créer ou ouvrir un projet temporaire de labo.

Aller dans l’onglet Proxy.

Vérifier que la page Intercept est visible et que l’état Intercept is off apparaît (au début, c’est volontaire).

Pourquoi ? Un proxy d’observation ne doit pas bloquer le trafic tant que la configuration n’est pas validée.

À observer : L’onglet HTTP history est présent (liste des requêtes). Le bouton d’interception est accessible.

Erreurs fréquentes : Démarrer avec l’interception activée et bloquer toute navigation. Se perdre entre “Proxy”, “Target”, “Repeater” sans avoir vérifié la capture.

Remarque / Point de vigilance : Un environnement de labo doit rester stable : si l’outil bloque le trafic, l’expérience devient frustrante et non reproductible.

Étape 2 — Vérifier le Proxy Listener (adresse et port)
Actions à réaliser :

Dans Burp, ouvrir Proxy settings ou Proxy listeners (selon la version).

Vérifier qu’un listener est actif (case Enabled ou Running cochée).

Noter le Port du proxy : <PORT_PROXY> (ex: 8080).

Noter l'Adresse d’écoute : “Loopback only” (127.0.0.1) ou “All interfaces” (*) selon le lab.

Pourquoi ? Le téléphone/émulateur doit savoir où envoyer le trafic (hôte + port).

À observer : Un listener “running”. Un port défini (ex. 8080, mais garder <PORT_PROXY> dans le document).

Erreurs fréquentes : Listener désactivé. Listener limité à “loopback” alors que l’émulateur n’est pas vu comme “local” (nécessite All interfaces si on utilise l'IP LAN).

Remarque / Point de vigilance : L’adresse et le port doivent être considérés comme des paramètres de labo, à documenter pour reproduire la séance.

Étape 3 — Identifier l’adresse réseau de la machine hôte
Objectif : Déterminer une adresse réseau atteignable depuis l’émulateur.

Actions à réaliser :

Sur la machine hôte, afficher l’adresse IP du réseau local utilisé par le labo (via ipconfig ou ip a).

Noter l’IP sous la forme <IP_HOTE> (ex: l'alias 10.0.2.2 pour l'AVD, ou l'IP locale 192.168.x.x).

Pourquoi ? Le proxy est sur la machine hôte. L’émulateur doit pouvoir la joindre via une adresse réseau valide.

À observer : Une adresse IPv4 de type A.B.C.D. Un réseau cohérent avec l’émulateur (même environnement réseau de labo).

Erreurs fréquentes : Utiliser une IP d’un autre réseau (VPN, interface inactive). Confondre IP publique et IP locale.

Remarque / Point de vigilance : En laboratoire, l’idéal est un réseau simple et isolé. Moins il y a d’interfaces réseau, moins il y a d’erreurs.

Étape 4 — Configurer le proxy côté Android Emulator
Actions à réaliser :

Dans l’Android Emulator, ouvrir les paramètres réseau (Wi-Fi).

Ouvrir les détails du réseau Wi-Fi actif (AndroidWifi).

Ouvrir les options avancées et choisir Proxy : Manual.

Renseigner :

Proxy hostname : <IP_HOTE> (ex: 10.0.2.2)

Proxy port : <PORT_PROXY> (ex: 8080)

Enregistrer.

Pourquoi ? Sans proxy configuré, le trafic du navigateur part directement vers Internet et n’apparaît pas dans Burp.

À observer : Le proxy est affiché en mode “Manual”. L’hôte et le port correspondent à ceux notés aux étapes 2 et 3.

Erreurs fréquentes : Mauvais port. Hostname vide. Proxy configuré sur un autre réseau Wi-Fi que celui actif.

Remarque / Point de vigilance : Le proxy configuré à ce niveau s’applique généralement au trafic du navigateur et de nombreuses applis. En labo, limiter le test à une cible autorisée.

Étape 5 — Premier test : capturer du HTTP (validation de base)
Objectif : Valider la chaîne de capture avec un trafic simple.

Actions à réaliser :

Dans l’émulateur, ouvrir le navigateur.

Accéder à une cible autorisée (idéalement une page de test interne ou une cible d’entraînement en clair, ex: http://neverssl.com).

Revenir dans Burp, ouvrir HTTP history.

Vérifier qu’au moins une requête apparaît.

Pourquoi ? Cette étape confirme que le routage via proxy fonctionne avant d’aborder HTTPS.

À observer : Une ligne par requête dans l’historique. Méthode (GET/POST), URL, statut, taille.

Erreurs fréquentes : Aucun trafic dans l’historique : proxy mal configuré ou listener inaccessible. Interception activée et requêtes bloquées.

Remarque / Point de vigilance : Si rien n’apparaît, revenir aux étapes 2–4 (listener, IP, proxy Android).

Étape 6 — Exporter le certificat CA de Burp
Actions à réaliser :

Dans Burp (onglet Proxy settings), cliquer sur Import / export CA certificate.

Choisir Certificate in DER format.

Sauvegarder le fichier sur la machine hôte sous le nom cacert.der.

Pourquoi ? Pour analyser le HTTPS, Android doit reconnaître Burp comme une autorité de confiance. Il faut d'abord extraire la "carte d'identité" de Burp.

À observer : La création réussie du fichier sur l'ordinateur.

Erreurs fréquentes : Exporter la clé privée au lieu du certificat seul.

Remarque / Point de vigilance : Ce fichier servira de base matérielle pour l'étape 8.

Étape 7 — Interception contrôlée (mode pédagogique)
Actions à réaliser :

Activer l’interception uniquement pour une courte séquence (passer sur Intercept is on).

Dans le navigateur, rafraîchir une page autorisée.

Observer Burp : la requête est “en attente”.

Désactiver l’interception après observation (repasser sur Intercept is off) ou cliquer sur Forward.

Pourquoi ? Comprendre le principe “proxy = point de passage” sans transformer le lab en manipulation agressive.

À observer : Différence entre “passif” (history) et “actif” (intercept).

Erreurs fréquentes : Laisser l’intercept activé et bloquer tout le trafic. Utiliser l’intercept pour modifier des requêtes (hors objectif de ce lab).

Remarque / Point de vigilance : L’interception est un outil puissant. En formation débutant, la priorité est la lecture et la preuve.

Étape 8 — HTTPS en laboratoire : principe du certificat CA (sans contournement)
Actions à réaliser :

Observer l’écran “Install a certificate” dans l’émulateur (dans les paramètres de sécurité).

Identifier les types proposés (CA certificate, VPN & app user certificate, Wi-Fi certificate).

Transférer et installer le certificat exporté à l'Étape 6.

Pourquoi ? HTTPS protège contre l’interception. Un proxy d’analyse en labo nécessite un modèle de confiance contrôlé, sinon le navigateur refuse la connexion (erreur de certificat).

À observer : La distinction entre certificat “CA” et certificats utilisateur/VPN/Wi-Fi. Les avertissements système lors de l’ajout d’une autorité de confiance.

Erreurs fréquentes : Installer un certificat de labo sur un téléphone personnel. Oublier de retirer le certificat à la fin.

Remarque / Point de vigilance : Un certificat CA de labo augmente la capacité d’observation, mais réduit la sécurité de l’environnement. Son usage doit être temporaire, documenté, et strictement limité au lab.
