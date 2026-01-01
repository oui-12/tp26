<img width="978" height="646" alt="Image" src="https://github.com/user-attachments/assets/a3593312-beb3-4aa7-895f-1aa8e9217b75" />
<img width="965" height="386" alt="Image" src="https://github.com/user-attachments/assets/d8fdbfd4-f3aa-4137-851e-e22dcd18d0d2" />
<img width="976" height="436" alt="Image" src="https://github.com/user-attachments/assets/f4c55300-92b7-479c-8ad7-777e283a4f71" />
<img width="1052" height="612" alt="Image" src="https://github.com/user-attachments/assets/79526220-5bfd-4e86-a378-77fe1aea2302" />

# 🚀 Microservice Observabilité et Résilience avec Spring Boot

Ce projet démontre l'implémentation d'un microservice Spring Boot moderne avec des fonctionnalités avancées d'observabilité, de résilience et d'intégration avec MySQL.

## 🛠️ Fonctionnalités Principales

- **🔍 Actuator** : Surveillance complète du microservice avec des métriques en temps réel
  - Suivi des performances
  - Métriques d'application
  - Informations sur les beans Spring
  - Configuration des variables d'environnement

- **🛡️ Resilience4j** : Gestion robuste des pannes et des timeouts
  - Circuit breaker pour prévenir les défaillances en cascade
  - Rate limiting pour protéger les ressources
  - Bulkheading pour isoler les défaillances
  - Retry avec backoff exponentiel

- **⚙️ Profils Spring** : Configuration flexible pour différents environnements
  - `dev` : Configuration optimisée pour le développement
  - `prod` : Configuration sécurisée pour la production
  - `test` : Configuration pour les tests automatisés

- **💓 Health Checks** : Points de terminaison de santé personnalisés
  - Vérification de la connexion à la base de données
  - État des services externes
  - Métriques de performance

- **⏱️ Stratégie d'attente** : Gestion intelligente des dépendances
  - Détection automatique des services disponibles
  - Reconfiguration à chaud des paramètres
  - Logs détaillés pour le débogage

## 📋 Prérequis

- ☕ **Java 11+** - Runtime Java requis
- 🧰 **Maven 3.6+** - Gestion des dépendances et build
- 🗃️ **MySQL 8.0+** - Base de données relationnelle
- 🐳 **Docker** (optionnel) - Pour l'exécution en conteneur
- 🔌 **Lombok** - Pour réduire le code boilerplate (installer le plugin dans votre IDE)

## ⚙️ Configuration

### 🔌 Base de données
1. Modifiez le fichier `application.yml` avec vos paramètres :
   ```yaml
   spring:
     datasource:
       url: jdbc:mysql://localhost:3306/votre_base
       username: utilisateur
       password: mot_de_passe
   ```
2. Le schéma est généré automatiquement via Hibernate
3. Les scripts SQL sont exécutés au démarrage (si activés)

### 🔄 Profils Spring
- **`dev`** : Configuration de développement
  - Accès H2 Console: `/h2-console`
  - Détails des requêtes SQL dans les logs
  - Auto-reload activé

- **`prod`** : Configuration production
  - Cache activé
  - Compression des réponses
  - Sécurité renforcée

- **`test`** : Configuration des tests
  - Base de données en mémoire H2
  - Données de test automatiques
  - Désactivation des fonctionnalités non essentielles

## 📊 Points de Terminaison d'Actuator

### 🔍 Vérification de l'état de santé
```http
GET /actuator/health
```
Retourne l'état global de santé de l'application, y compris :
- État de la base de données
- Espace disque disponible
- État du cache
- Services personnalisés

### ✅ Vérification de la disponibilité
```http
GET /actuator/health/readiness
```
Indique si l'application est prête à recevoir du trafic :
- Vérifie les connexions aux services critiques
- S'assure que le démarrage est terminé
- Utile pour les orchestrateurs de conteneurs

### 📈 Métriques d'application
```http
GET /actuator/metrics
```
Fournit des métriques détaillées sur :
- Utilisation du CPU et mémoire
- Temps de réponse des requêtes
- Nombre de requêtes par endpoint
- Taux d'erreur

## 💰 API des Prix

### 📥 Obtenir un prix par ID
```http
GET /api/prices/{id}
```
**Paramètres :**
- `id` : Identifiant unique du produit (requis)

**Réponse réussie (200 OK) :**
```json
{
  "id": 1,
  "productId": "PRD-001",
  "amount": 99.99,
  "currency": "EUR",
  "validUntil": "2025-12-31"
}
```

**Gestion des erreurs :**
- `404 Not Found` : Produit non trouvé
- `400 Bad Request` : ID invalide
- `500 Internal Server Error` : Erreur serveur

### 🧪 Tester la résilience (simule une erreur)
```http
GET /api/prices/1?fail=true
```
**Paramètres :**
- `fail` : Si `true`, simule une erreur pour tester la résilience

**Comportement :**
1. Active le circuit breaker
2. Enregistre les métriques de résilience
3. Fournit une réponse de secours si configuré

## 🔧 Dépannage

### 🔌 Problèmes de connexion à la base de données
- Vérifiez que MySQL est en cours d'exécution
- Vérifiez les identifiants dans `application.yml`
- Consultez les logs pour les erreurs de connexion

### ❌ Erreurs 500
- Activez le mode debug dans `application.yml` :
  ```yaml
  logging:
    level:
      root: DEBUG
  ```
- Vérifiez la pile d'appels complète dans les logs

### 🛠️ Problèmes de résilience
- Vérifiez la configuration dans `resilience4j.circuitbreaker.instances`
- Consultez les métriques sur `/actuator/circuitbreakers`
- Vérifiez les timeouts dans la configuration

## 🚀 Améliorations Futures

### 🧪 Tests
- [ ] Ajouter des tests d'intégration pour les contrôleurs
- [ ] Implémenter des tests de charge avec JMeter
- [ ] Ajouter des tests de résilience

### 📊 Observabilité
- [ ] Intégrer Sleuth/Zipkin pour la journalisation distribuée
- [ ] Ajouter des métriques personnalisées avec Micrometer
- [ ] Implémenter des alertes basées sur les métriques

### 🔄 Évolutivité
- [ ] Ajouter le support du caching avec Redis
- [ ] Implémenter l'API GraphQL
- [ ] Ajouter la pagination pour les collections

### 🔒 Sécurité
- [ ] Implémenter l'authentification JWT
- [ ] Ajouter la validation des entrées
- [ ] Mettre en place le rate limiting global

## 📄 Licence

Ce projet est sous licence [MIT](LICENSE).

```
MIT License

Copyright (c) 2025 Votre Nom

Permission est accordée, à toute personne obtenant une copie
de ce logiciel et des fichiers de documentation associés (le "Logiciel"), de traiter
dans le Logiciel sans restriction, y compris sans limitation les droits
d'utilisation, de copie, de modification, de fusion, de publier,
distribuer, sous-licencier et/ou vendre des copies du Logiciel, et de
permettre aux personnes à qui le Logiciel est fourni de le faire, sous réserve
des conditions suivantes :

L'avis de droit d'auteur ci-dessus et cet avis d'autorisation doivent être inclus dans
toutes les copies ou parties substantielles du Logiciel.

LE LOGICIEL EST FOURNI "TEL QUEL", SANS GARANTIE D'AUCUNE SORTE, EXPLICITE OU
IMPLICITE, Y COMPRIS MAIS SANS S'Y LIMITER LES GARANTIES DE QUALITÉ MARCHANDE,
D'ADÉQUATION À UN USAGE PARTICULIER ET DE NON-VIOLATION. EN AUCUN CAS LES
AUTEURS OU TITULAIRES DE DROITS D'AUTEUR NE POURRONT ÊTRE TENUS POUR RESPONSABLES DE TOUTE RÉCLAMATION, DOMMAGE OU AUTRE RESPONSABILITÉ, QUE CE SOIT DANS UNE ACTION DE CONTRAT, DE DÉLIT OU AUTRE, DÉCOULANT DE, EN LIEN AVEC OU EN RAPPORT AVEC LE LOGICIEL OU SON UTILISATION, OU D'AUTRES OPÉRATIONS EFFECTUÉES AVEC LE LOGICIEL.
```
