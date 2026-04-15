# LAB 18 – FireStorm : solution détaillée pas à pas

## Présentation

Dans ce laboratoire, l’objectif est d’analyser l’application Android **FireStorm** afin d’en comprendre le fonctionnement et d’en extraire les informations nécessaires pour résoudre le challenge.

L’analyse repose sur plusieurs approches complémentaires :
- reverse engineering de l’application ;
- observation du comportement en temps réel ;
- exploitation des services Firebase ;
- récupération du flag final.

---

## Outils utilisés

Pour mener cette analyse, les outils suivants ont été utilisés :

- **ADB** pour communiquer avec l’appareil Android ;
- **Ghidra** pour examiner le code compilé ;
- **Frida** pour instrumenter l’application dynamiquement ;
- **Python / Pyrebase** pour interagir avec Firebase ;
- **Émulateur Android** pour tester l’application dans un environnement contrôlé.

---

## Étape 1 – Analyse de la logique du mot de passe

L’étude du code de l’application met en évidence une fonction appelée `Password()`.

<img width="1898" height="968" alt="image" src="https://github.com/user-attachments/assets/98584542-c13c-4345-9367-a12330c19019" />


Cette fonction construit un mot de passe en combinant plusieurs éléments :
- des chaînes statiques définies dans les ressources (`strings.xml`) ;
- une partie générée dynamiquement via une fonction native `generateRandomStrings` issue de la bibliothèque `libfirestorm.so`.
<img width="1106" height="268" alt="un mot de passe" src="https://github.com/user-attachments/assets/7f56fe12-6244-403d-8d5c-c81908de7243" />



Le fichier de ressources contient également des informations sensibles, notamment :
- une adresse email utilisée pour l’authentification ;
- des paramètres Firebase (API key, URL de base de données, etc.).

Un point important est que cette fonction de génération de mot de passe n’est pas directement appelée par l’interface utilisateur, ce qui nécessite une approche dynamique pour en extraire la valeur.

---

## Étape 2 – Préparation de l’instrumentation avec Frida

Pour observer le comportement de l’application en cours d’exécution, on met en place **Frida**.
<img width="1953" height="390" alt="1" src="https://github.com/user-attachments/assets/d858cbe0-ebba-498f-9dbd-c7d46ca849c7" />



Une fois configuré, Frida permet d’injecter du code et d’intercepter les fonctions internes de l’application.

<img width="1532" height="418" alt="ChatGPT Image Apr 15, 2026, 10_08_01 PM" src="https://github.com/user-attachments/assets/409a878f-b3f9-4bbe-9dae-4b14e4050736" />


---

## Étape 3 – Extraction du mot de passe

Un script Frida est utilisé pour accéder à la fonction responsable de la génération du mot de passe.
<img width="1600" height="488" alt="WhatsApp Image 2026-04-15 at 22 31 47" src="https://github.com/user-attachments/assets/3b5ae1fb-8fb4-45e2-8ced-138648fce8be" />


Cette méthode permet de récupérer la valeur complète du mot de passe :
C7_dotpsC7t7f_._In_i.IdttpaofoaIIdIdnndIfC
## Étape 4 – Exploitation de Firebase

Avec les identifiants obtenus, il est possible d’interagir avec Firebase à l’aide d’un script Python.

Cette étape permet d’accéder aux données protégées et de récupérer le flag du challenge.

## Conclusion

Ce challenge met en avant l’importance de combiner plusieurs techniques :

analyse du code ;
reverse engineering ;
instrumentation dynamique ;
exploitation de services externes.

Même si certaines fonctions critiques ne sont pas directement utilisées dans l’application, elles restent exploitables via des outils comme Frida, ce qui permet de reconstituer les informations nécessaires pour atteindre l’objectif final.
