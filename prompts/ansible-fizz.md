
# L'Ansible Fizz

## Métadonnées

| Champ | Valeur |
|-------|--------|
| **Auteur** | Pierre Hilson |
| **Description** | Transformer un référentiel technique complexe en documentation structurée et prête à l'emploi, avec l'effervescence de l'automatisation. |
| **UseCase** | Passer d'un repository Ansible non documenté à trois documents professionnels distincts (Architecture & Sécurité, Structure Ansible & Conventions, Guide d'installation du serveur Ansible) permettant une présentation de haut niveau, la standardisation et l'onboarding rapide. |
| **Thème** | Rédaction |
| **Sous-thème** | Rédaction assistée |
| **Plateforme** | Possible avec Claude (Claude Sonnet 4 ou supérieur recommandé) ou ChatGPT-5. Plus adapté à un agent (ex: Claude Code). |

---

## Contenu du prompt structuré

### Objectif global

Analyser un repository Ansible (fourni sous forme de zip) et produire trois documents Markdown :

1. **Architecture & Sécurité** (niveau conceptuel, importable dans Confluence).
2. **Repository Ansible : structure & conventions**.
3. **Guide d'installation et d'utilisation d'Ansible** (avec exemples de commandes).

---

### Contexte

- Plateforme basée sur **RHEL9**, **Apigee Private Cloud**, **ElasticSearch**.
- Repository Ansible structuré selon les conventions **Ansible Lint**.
- **Environnements** :  
  - sandbox (1 host)  
  - dev (10 hosts)  
  - test (10 hosts)  
  - valid (24 hosts)  
  - prod (24 hosts)
- Inventaires et rôles Ansible écrits pour être **idempotents** et **scalables**.
- Secrets gérés via **Ansible Vault** (fichiers `secrets.yml` encryptés).
- Répertoire `_archive` et playbooks `z_*.yml` à ignorer pour l'analyse, mais `z_*.yml` doivent être listés en annexe avec une description courte.

---

### Contraintes

1. Ignorer le répertoire `_archive`.
2. Ne pas inclure les playbooks `z_*.yaml` dans l'analyse de l'architecture ; les lister en annexe avec une brève description.
3. Les fichiers `secrets.yml` sont encryptés via **Ansible Vault**. Déduire que les variables sensibles référencées dans les tasks sont définies dans ces fichiers.
4. **Langue** : Français.
5. **Ansible Lint** : mentionner qu'il est utilisé, qu'il doit être exécuté avant tout commit, et qu'il peut être configuré dans l'IDE.
6. **Script secrets** : `bin/encrypt_all_secrets.sh` doit être utilisé obligatoirement avant tout commit.
7. **Niveau d'abstraction** :  
   - Tâche 1 (Architecture) : conceptuel (pas trop de détails opérationnels).  
   - Inclure les bonnes pratiques appliquées (**Infrastructure as Code**, objectif **Zero Trust** sans prétendre qu'il est implémenté partout).  
   - Vue globale des environnements avec différences majeures.
8. **Précédence des variables** : expliquer l'ordre et insister sur l'usage de `defaults` dans les rôles (et non `vars`).
9. **Vérifications obligatoires** :  
   - Avant de commencer : vérifier si des questions subsistent. Si tout est clair, annoncer que tu démarres.  
   - À la fin de chaque tâche : inclure une checklist confirmant que tous les points requis sont couverts.

---

### Livrables

#### Tâche 1 — Document "Architecture & Sécurité" (Markdown, importable dans Confluence)

- Sans Mermaid.
- **Intro** : annoncer la structure (partie exécutive + technique).
- **Partie exécutive** : objectifs, portée, bénéfices (IaC, objectif Zero Trust), principes clés.
- **Partie technique** :  
  - Vue globale des composants (**Apigee : MS, RMP, DS, QS, PS ; ElasticSearch ; Logstash ; Filebeat ; Kibana**).  
  - Distribution des fonctions par environnement (différences majeures).  
  - **Sécurité** :  
    - Gestion des utilisateurs/groupes.  
    - SSH (accès, clés).  
    - SSL/TLS (certificats, keystores).  
    - Secrets (Ansible Vault, `secrets.yml`).  
    - Objectif Zero Trust (mentionner comme objectif, pas comme implémentation complète).  
  - Bonnes pratiques (**IaC**, idempotence, séparation inventaires/roles, tagging).
- **Annexe** : liste des playbooks `z_*.yaml` avec description courte.
- **Checklist fin de tâche** : vue globale, distribution, sécurité, bonnes pratiques, annexe.

---

#### Tâche 2 — Document "Repository Ansible : structure & conventions" (Markdown, Mermaid autorisé)

- Décrire la structure (inventaires, `group_vars`, `roles`, `playbooks`, `bin`).
- Expliquer où trouver quoi (variables, inventaires, rôles).
- Expliquer la précédence des variables (ordre complet + insister sur `defaults` dans les rôles).
- Décrire conventions de nommage (préfixes par composant, suffixes `_password`, `_user`).
- Mentionner **Ansible Lint** (utilisation + configuration dans IDE).
- Schémas Mermaid autorisés pour illustrer la structure.
- **Checklist fin de tâche** : structure, conventions, précédence, emplacement, mention Lint.

---

#### Tâche 3 — Document "Installer et utiliser un serveur Ansible sur RHEL9" (Markdown, Mermaid autorisé)

- **Installation sur RHEL9** :  
  - Pré-requis (packages).  
  - Installation Python + virtualenv.  
  - Installation Ansible (dernière version).
- **Fichier `.ansible_vault_pass.txt`** :  
  - Rôle, emplacement recommandé, permissions, non versionné.
- Mise à jour du serveur Ansible.
- **Exemples de commandes** (illustratifs + explications) :  
  1. Cloner le repo depuis `https://VOTRE-REPO`.  
  2. Exécuter un rôle sur un environnement.  
  3. Exécuter une commande arbitraire sur tous les hôtes d'un groupe.  
  4. Générer un CSV des hôtes du groupe `logstash` en `valid` (avec environnement, IP, nom Ansible, fonctions).  
  5. Créer un nouveau rôle (`ansible-galaxy init`).  
  6. Vérifier/encrypter les secrets avec `bin/encrypt_all_secrets.sh`.
- **Checklist fin de tâche** : installation, venv, Ansible, vault pass, mise à jour, exemples demandés, encryption des secrets.

---

### Méthode de travail exigée

1. Avant de commencer : poser toutes les questions nécessaires. Sinon, annoncer que tu démarres.
2. Analyser le zip : inventorier structure, inventaires, `group_vars`, rôles, scripts.
3. Déduire conventions et bonnes pratiques.
4. Produire les 3 documents selon les contraintes.
