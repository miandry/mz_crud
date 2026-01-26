# mz_crud – Drupal CRUD API Module

`mz_crud` est un module Drupal qui expose une API REST complète pour effectuer
des opérations CRUD (Create, Read, Update, Delete) sur des contenus Drupal,
en prenant en charge :

- Champs standards (title, body, etc.)
- Paragraphs
- Media (images, vidéos, fichiers)
- Utilisation comme backend Headless CMS pour applications mobiles et web

---

## 📦 Installation

```bash
cd web/modules/custom
git clone https://github.com/miandry/mz_crud.git
drush en mz_crud -y
```

Modules requis :
- Paragraphs
- Media
- REST
- Serialization

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

## 🌐 Base URL

```
/api/mz_crud
```

Headers communs :
```
Content-Type: application/json
Accept: application/json
```

---

## 📚 LISTE COMPLÈTE DES ENDPOINTS

| Méthode | Endpoint                  | Description |
|--------|---------------------------|------------|
| POST   | /api/mz_crud/create       | Créer un contenu |
| GET    | /api/mz_crud/{id}         | Lire un contenu |
| GET    | /api/mz_crud/list         | Lister tous les contenus |
| GET    | /api/mz_crud/list/{type}  | Lister par type de contenu |
| PUT    | /api/mz_crud/{id}         | Mettre à jour |
| DELETE | /api/mz_crud/{id}         | Supprimer |
| POST   | /api/mz_crud/media        | Créer un Media |
| GET    | /api/mz_crud/media/{id}   | Lire un Media |
| DELETE | /api/mz_crud/media/{id}   | Supprimer un Media |

---

# 🟢 CREATE – Créer un contenu

```
POST /api/mz_crud/create
```

```json
{
  "type": "article",
  "title": "Article avec Paragraphs et Media",
  "body": {
    "value": "Ceci est le contenu principal",
    "format": "basic_html"
  },
  "paragraphs": [
    {
      "type": "text_block",
      "fields": {
        "field_text": {
          "value": "Ceci est un paragraphe texte",
          "format": "basic_html"
        }
      }
    },
    {
      "type": "image_block",
      "fields": {
        "field_image": {
          "media_id": 5
        },
        "field_caption": "Une image principale"
      }
    }
  ]
}
```

Réponse :
```json
{
  "status": "success",
  "node_id": 45
}
```

---

# 🔵 READ – Lire un contenu

```
GET /api/mz_crud/45
```

```json
{
  "id": 45,
  "type": "article",
  "title": "Article avec Paragraphs et Media",
  "body": "Ceci est le contenu principal",
  "paragraphs": []
}
```

---

# 📃 LIST – Lister tous les contenus

```
GET /api/mz_crud/list
```

```json
[
  { "id": 1, "title": "Article 1", "type": "article" },
  { "id": 2, "title": "Page 1", "type": "page" }
]
```

---

# 📂 LIST BY TYPE

```
GET /api/mz_crud/list/article
```

```json
[
  { "id": 1, "title": "Article 1" },
  { "id": 2, "title": "Article 2" }
]
```

---

# 🟡 UPDATE

```
PUT /api/mz_crud/45
```

```json
{
  "title": "Titre mis à jour"
}
```

---

# ⚫ DELETE

```
DELETE /api/mz_crud/45
```

```json
{
  "status": "deleted",
  "node_id": 45
}
```

---

# 🖼 MEDIA – Création

```
POST /api/mz_crud/media
```

Form-data :
| Key | Value |
|------|------|
| file | fichier |
| type | image |
| name | photo |

Réponse :
```json
{
  "status": "success",
  "media_id": 5,
  "url": "/sites/default/files/2026-01/photo.jpg"
}
```

---

# ⚠️ Notes importantes

- Le champ `field_paragraphs` doit être :
  Entity reference revisions → Paragraph
- Les machine names doivent correspondre exactement à la configuration Drupal.
- Les Media doivent exister avant d’être utilisés dans les Paragraphs.
- Toutes les réponses sont en JSON.

---

# 🚀 Objectif

Faire de Drupal un Headless CMS complet capable de servir :
- des applications mobiles
- des applications web
- des systèmes externes via API
