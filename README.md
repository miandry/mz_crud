# mz_crud – Drupal CRUD API Module

`mz_crud` est un module Drupal qui expose une API REST complète pour effectuer
des opérations CRUD (Create, Read, Update, Delete) sur des contenus Drupal,
en prenant en charge :

- Champs standards (title, body, etc.)
- Paragraphs
- Media (images, vidéos, fichiers)

---

## 📦 Installation

```bash
cd web/modules/custom
git clone https://github.com/miandry/mz_crud.git
drush en mz_crud -y
```

---

## 🔐 Permissions & Authentification

Permission minimale requise :
```
access content
```

Méthodes d’authentification possibles :
- Session Drupal (cookie)
- Basic Auth
- Bearer Token (JWT / Simple OAuth)

---

Headers communs :
```
Content-Type: application/json
Accept: application/json
```

---


# 🟢 CREATE – Créer un contenu

```
POST /crud/save

```

```json
{
  "entity_type": "node",
  "title": "Article avec Paragraphs et taxonomy",
  "bundle": "article",
  "field_article_title": "Article 1",
  "field_categories": [ // champ de reférence de type paragraph
    {
      "field_type": "type 1",
      "field_name": "category 1"
    },
    {
      "field_type": "type 2",
      "field_name": "category 2"
    },
  ],
  "field_color": 3 //tid du taxonomy
}
```

Réponse :
```json
{
  "item": 45,
  "status": true
}
```

---

# 🟡 UPDATE

⚠️ Le node_id (nid) est obligatoire pour l’opération de mise à jour.

```
POST /crud/save
```

```json
{
  "entity_type": "node",
  "title": "Article modifié avec Paragraphs et taxonomy",
  "bundle": "article",
  "nid": 12, // requis pour la mise a jour
  "field_article_title": "Article 1 modifié",
  "field_categories": [ // champ de reférence de type paragraph
    {
      "field_type": "type 1",
      "field_name": "category 1"
    },
    {
      "field_type": "type 2",
      "field_name": "category 2"
    },
  ],
  "field_color": 3 //tid du taxonomy
}
```


---


# 🟢 Inscription

Endpoint pour créer un nouvel utilisateur via l’API.

```
POST /crud/register
```

```json
{
  "name": "username",
  "pass": "password123"
}
```

Réponse :
```json
{
    "status": 1,
    "name": "username",
    "token": "c7WhShh8hXkiRyq...",
    "id": "27"
}
```

---


# 🟡 Authentification

Endpoint pour authentifier un utilisateur via l’API.

```
POST /crud/login
```

```json
{
  "name": "username",
  "password": "password123"
}
```

Réponse :
```json
{
    "status": true,
    "name": "username",
    "token": "HVfdjAg2T-ElU8cJQXSTnKsd2nvOpjqAtdY31TlQNPg",
    "roles": ["authenticated"],
    "id": "27"
}
```

---

# ⚠️ Notes importantes

- Le champ `field_paragraphs` doit être :
  Entity reference revisions → Paragraph
- Les machine names doivent correspondre exactement à la configuration Drupal.
- Toutes les réponses sont en JSON.

---

# 🚀 Objectif

Faire de Drupal un Headless CMS complet capable de servir :
- des applications mobiles
- des applications web
- des systèmes externes via API
