# TP 15 : Service GraphQL avec Spring Boot

## **Description**
API GraphQL pour gérer des comptes bancaires (courant/épargne) et leurs transactions (dépôt/retrait) avec calcul automatique des soldes.

## **Technologies**
- Spring Boot 2.7+
- Spring GraphQL
- Spring Data JPA
- H2 Database (base en mémoire)
- Maven/Gradle

## **Installation**
```bash
# 1. Cloner le projet
git clone [url-du-projet]

# 2. Installer les dépendances
mvn clean install

# 3. Lancer l'application
mvn spring-boot:run
```

## **URLs d'accès**
- **GraphiQL** : http://localhost:8080/graphiql (interface de test)
- **API GraphQL** : http://localhost:8080/graphql
- **H2 Console** : http://localhost:8080/h2-console (base de données)

## **Configuration**
Fichier `application.properties` :
```properties
server.port=8080
spring.graphql.graphiql.enabled=true
spring.h2.console.enabled=true
```

## **Schéma GraphQL**

### **Types principaux**
```graphql
type Compte {
  id: ID
  solde: Float
  type: TypeCompte
  dateCreation: String
}

type Transaction {
  id: ID
  montant: Float
  date: String
  type: TypeTransaction
  compte: Compte
}
```

### **Énumérations**
```graphql
enum TypeCompte { COURANT, EPARGNE }
enum TypeTransaction { DEPOT, RETRAIT }
```

## **Requêtes disponibles**

### **Créer un compte**
```graphql
mutation {
  saveCompte(compte: {
    solde: 1000.0,
    dateCreation: "2024-01-15",
    type: COURANT
  }) {
    id
    solde
    type
  }
}
```

### **Ajouter une transaction**
```graphql
mutation {
  addTransaction(transaction: {
    compteId: 1,
    montant: 500.0,
    date: "2024-01-16",
    type: DEPOT
  }) {
    id
    montant
    compte { id solde }
  }
}
```

### **Récupérer les transactions d'un compte**
```graphql
query {
  compteTransactions(id: 1) {
    id
    montant
    date
    type
  }
}
```

### **Statistiques**
```graphql
query {
  totalSolde { count sum average }
  transactionStats { count sumDepots sumRetraits }
}
```

## **Structure du projet**
```
src/main/java/
├── entity/       (Compte, Transaction)
├── repository/   (Spring Data JPA)
├── controller/   (GraphQL resolvers)
└── service/      (Logique métier)
```

## **Auteurs**

Réalisé par : Ettouyjer yasmine.

Encadré par : Mr.Mohamed Lechgar.


