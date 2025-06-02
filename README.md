## Cahier des charges technique - Fast Track (MVP Accès Marchés Publics)

> ⚠️ **Document de travail – première piste pour ouvrir la discussion**  
> Toutes les hypothèses (scope, techno, planning) seront challengées et re-validées collectivement pendant la phase d’investigation avec l’équipe produit et les utilisateurs.

## Objectif du projet

Créer un **MVP (Minimum Viable Product)** permettant aux TPE/PME de candidater à des marchés publics en utilisant uniquement leur **numéro SIRET**. L'utilisateur entre son numéro SIRET, le système récupère les informations pertinentes via **API Entreprise**, les affiche, puis permet la validation et la soumission.

**Livraison du MVP : septembre 2025**

---

## Architecture technique recommandée

### 1. Environnement full **JavaScript / TypeScript** unifié

Pourquoi ce choix ?

- **Gain de temps** : Utiliser des frameworks modernes (**Next.js**, **Nest.js**) qui intègrent déjà des optimisations avancées (perfs, SEO, accessibilité), sans avoir à réinventer la roue.  
- **Productivité et montée en compétence** : Un seul langage (**TypeScript**) pour tout le projet réduit les frictions et accélère l’onboarding des développeurs.  
- **Technologies éprouvées** : Écosystème mature, documentation riche, et forte communauté.  
- **Adapté au MVP** : Cette stack est conçue pour être mise en place rapidement, tout en garantissant des bases solides et évolutives.

> 💬 **Co-construction**  
> Les briques listées ci-dessous sont une proposition initiale.  
> Si l’exploration révèle qu’une composante existante (**Ruby on Rails**, **Python**) couvre déjà le besoin, nous l’intégrerons plutôt que de ré-inventer.

### 2. Stack technique détaillée

#### Frontend (Interface utilisateur)

- **Next.js** (App Router, TypeScript) : Framework moderne pour un front rapide, optimisé et orienté SEO. Il gère automatiquement le SSR/SSG, les optimisations des performances (Core Web Vitals), l’accessibilité et le SEO dès le départ.  
- **Tailwind CSS + DSFR** : Rapidité de mise en place des interfaces, tout en respectant le design system de l’État (DSFR) et les normes d’accessibilité (RGAA).  
- **React Hook Form + Zod** → validation performante et typée.  
- **Tests** : **Playwright** (E2E), **Jest** (unitaires) – utilisés de manière ciblée pour valider les parcours clés du MVP.  
- **Accessibilité** : Intégrée dans le design (DSFR), auditée en continu.

#### Backend (API et logique métier)

- **Nest.js** (TypeScript) : Framework modulaire qui permet de créer rapidement une API REST sécurisée et structurée. Il simplifie l’ajout de fonctionnalités (middleware, validation, sécurité).  
- **API Proxy** : Intermédiaire entre le frontend et l’API Entreprise.  
  - **Pourquoi ?** Pour simplifier l’intégration (le frontend n’a pas à gérer les subtilités de l’API Entreprise), sécuriser les appels (pas d’exposition de clés ou d’endpoints sensibles), gérer les quotas et les erreurs (fallbacks, cache) et assurer la robustesse globale du système.  
- **REST (OpenAPI/Swagger)** : Standard simple et documenté, facile à comprendre pour les non-techniques, et interopérable avec d’autres services.

#### Base de données : **PostgreSQL** (via **Prisma**)

- Pourquoi PostgreSQL et pas NoSQL ? Données relationnelles simples, besoin de requêtes statistiques basiques, robustesse et standard reconnu.  
- **Prisma** : ORM moderne et typé TypeScript, facilite l’interaction avec la base, génère des types automatiques, simplifie la validation des données, et accélère le développement tout en limitant les erreurs.

#### Authentification

- **FranceConnect** : Authentification des entreprises via le représentant légal.  
- **JWT** ou sessions : Gestion des sessions côté API interne.

#### Déploiement

- **Docker** : Standard pour un environnement reproductible et portable.  
- **PaaS** (**Railway**, **Fly.io**, **Scalingo**) : Déploiement rapide sans configuration lourde.  
- **CI/CD (GitHub Actions)** : Automatisation des tests, linting, build, déploiement.  
- **Monitoring** : **Sentry**, logs JSON (Logtail/Datadog).

#### Sécurité

- Validation stricte des entrées (**Zod**, **class-validator**).  
- **CORS**, **rate limiting**, **Helmet** dès le départ.  
- Gestion des secrets via variables d’environnement.

---

## MVP - Parcours utilisateur détaillé

1. L’utilisateur saisit son **numéro SIRET**.  
2. Frontend (**Next.js**) → Backend (**Nest.js**) → **API Entreprise**.  
3. Données affichées (raison sociale, adresse, code APE, etc.).  
4. L’utilisateur valide ou modifie ses informations.  
5. Soumission du formulaire.  
6. Confirmation (et PDF récapitulatif optionnel).

**Cas prioritaires** :

- **SIRET valide/invalide**.  
- **API Entreprise non disponible** (fallback propre).  
- **Accessibilité** et responsive dès le départ.

---

## Technologies et outils

| Composant        | Technologie                         | Pourquoi                                      |
|------------------|-------------------------------------|-----------------------------------------------|
| **Frontend**     | **Next.js**, **Tailwind**, **DSFR** | Rapide, perfs intégrées, SEO, accessibilité   |
| **Backend**      | **Nest.js**                         | Robuste, modulaire, sécurisé                  |
| **Base de données** | **PostgreSQL (Prisma)**          | Données relationnelles, robustesse, typage TS |
| **Authentification** | **FranceConnect**, **JWT**      | Auth publique + API sécurisée                 |
| **Tests**        | **Playwright**, **Jest**            | Qualité et robustesse                         |
| **CI/CD**        | **GitHub Actions**                  | Automatisation, rapidité                      |
| **Monitoring**   | **Sentry**, logs JSON               | Observabilité                                 |
| **Déploiement**  | **Docker**, **Railway/Fly.io**      | Simplicité, rapidité                          |
| **Accessibilité**| **Pa11y**, audits RGAA              | Conforme secteur public                       |

---

## Pourquoi ce choix de stack et de méthode ?

Ce choix de stack permet de :

- **Aller vite** : utiliser des frameworks modernes qui font déjà le travail lourd (perfs, SEO, accessibilité) permet de livrer un MVP rapidement sans sacrifier la qualité.  
- **Minimiser les risques techniques** : solutions éprouvées, documentées, et largement adoptées.  
- **Faciliter l’onboarding et la maintenance** : **TypeScript unifié**, structure claire, code lisible.  
- **Garder une porte ouverte pour la suite** : cette base solide permet d’ajouter des fonctionnalités sans refondre l’existant.

En clair, c’est un **MVP pragmatique**, qui maximise le temps passé à livrer de la valeur à l’utilisateur au lieu de gérer des problèmes techniques.

---

## Mini-planning indicatif pour le MVP (en 3 mois)

| Période          | Livrables principaux                                                                         |
|------------------|----------------------------------------------------------------------------------------------|
| **Semaine 1-2**  | Setup stack (**Next.js**, **Nest.js**, **Docker**, **CI/CD**), connexion API Entreprise testée |
| **Semaine 3-4**  | UI formulaire SIRET + récupération données API Entreprise                                     |
| **Mois 2**       | Parcours complet : validation, soumission, retour utilisateur                                 |
| **Mois 3**       | Accessibilité RGAA, tests E2E ciblés, fallback API, PDF optionnel, optimisations perfs         |
| **Fin mois 3**   | MVP prêt pour mise en production et tests utilisateurs                                        |

---

## Glossaire

- **MVP (Minimum Viable Product)** : Version minimale d’un produit qui permet de répondre à un besoin utilisateur de manière fonctionnelle.  
- **API Proxy** : Intermédiaire qui permet de simplifier et sécuriser l’accès à une API externe (ici API Entreprise).  
- **Next.js** : Framework JavaScript/React optimisé pour le rendu côté serveur et le SEO.  
- **Nest.js** : Framework backend TypeScript inspiré d’Angular, structuré et modulaire.  
- **TypeScript** : Surcouche de JavaScript apportant le typage et la robustesse.  
- **PostgreSQL** : Base de données relationnelle robuste et open source.  
- **Prisma** : ORM TypeScript qui facilite la manipulation de bases relationnelles.  
- **FranceConnect** : Système d’identification sécurisé proposé par l’État français.  
- **JWT (JSON Web Token)** : Format standard pour sécuriser les échanges d’informations.  
- **CI/CD** : Intégration et déploiement continus (automatisation du cycle de développement).  
- **PaaS (Platform as a Service)** : Plateforme clé-en-main pour déployer facilement des applications.  
- **Core Web Vitals** : Indicateurs de performance web définis par Google (LCP, FID, CLS).

---

## Conclusion

Cette architecture vise à :

- **Aller vite** sans sacrifier la qualité : frameworks modernes, stack simple et efficace.  
- **Maximiser l’impact** : un produit utile, simple, performant et accessible dès le début.  
- **Être solide et évolutif** : une base saine qui pourra évoluer selon les besoins du projet.  

**Les décisions finales de stack et de calendrier seront prises ensemble après la phase d’investigation.**

Prêt à transformer cette vision en plan d’action concret.
