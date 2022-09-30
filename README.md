# Bank of Anthos

**Bank of Anthos** is a sample HTTP-based web app that simulates a bank's payment processing network, allowing users to create artificial bank accounts and complete transactions.


If you are using Bank of Anthos, please ★Star this repository to show your interest!

**Note to Googlers:** Please fill out the form at [go/bank-of-anthos-form](https://goto2.corp.google.com/bank-of-anthos-form).

## Screenshots

| Sign in                                                                                                        | Home                                                                                                    |
| ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| [![Login](./docs/login.png)](./docs/login.png) | [![User Transactions](./docs/transactions.png)](./docs/transactions.png) |


## Service architecture

![Architecture Diagram](./docs/architecture.png)

| Service                                          | Language      | Description                                                                                                                                  |
| ------------------------------------------------ | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| [frontend](./src/frontend)                       | Python        | Exposes an HTTP server to serve the website. Contains login page, signup page, and home page.                                                |
| [ledger-writer](./src/ledgerwriter)              | Java          | Accepts and validates incoming transactions before writing them to the ledger.                                                               |
| [balance-reader](./src/balancereader)            | Java          | Provides efficient readable cache of user balances, as read from `ledger-db`.                                                                |
| [transaction-history](./src/transactionhistory)  | Java          | Provides efficient readable cache of past transactions, as read from `ledger-db`.                                                            |
| [ledger-db](./src/ledger-db)                     | PostgreSQL | Ledger of all transactions. Option to pre-populate with transactions for demo users.                                                         |
| [user-service](./src/userservice)                | Python        | Manages user accounts and authentication. Signs JWTs used for authentication by other services.                                              |
| [contacts](./src/contacts)                       | Python        | Stores list of other accounts associated with a user. Used for drop down in "Send Payment" and "Deposit" forms. |
| [accounts-db](./src/accounts-db)                 | PostgreSQL | Database for user accounts and associated data. Option to pre-populate with demo users.                                                      |
| [loadgenerator](./src/loadgenerator)             | Python/Locust | Continuously sends requests imitating users to the frontend. Periodically creates new accounts and simulates transactions between them.      |


## Quickstart 


1. **Clone this repository.**

```
git clone https://github.com/vunetsystems/bank-of-anthos.git
cd bank-of-anthos/
```


2. **Deploy Bank of Anthos to the cluster.**

```
kubectl apply -f ./extras/jwt/jwt-secret.yaml
kubectl apply -f ./kubernetes-manifests
```

3. **Wait for the Pods to be ready.**

```
kubectl get pods
```

After a few minutes, you should see the Pods in a `Running` state:

```
NAME                                  READY   STATUS    RESTARTS   AGE
accounts-db-6f589464bc-6r7b7          1/1     Running   0          99s
balancereader-797bf6d7c5-8xvp6        1/1     Running   0          99s
contacts-769c4fb556-25pg2             1/1     Running   0          98s
frontend-7c96b54f6b-zkdbz             1/1     Running   0          98s
ledger-db-5b78474d4f-p6xcb            1/1     Running   0          98s
ledgerwriter-84bf44b95d-65mqf         1/1     Running   0          97s
loadgenerator-559667b6ff-4zsvb        1/1     Running   0          97s
transactionhistory-5569754896-z94cn   1/1     Running   0          97s
userservice-78dc876bff-pdhtl          1/1     Running   0          96s
```

4. **Access the web frontend in a browser** using the frontend's external IP.

```
kubectl get service frontend | awk '{print $4}'
```

Visit `https://EXTERNAL_IP` to access your instance of Bank of Anthos.


## Troubleshooting

See the [troubleshooting guide](./docs/troubleshooting.md) for resolving common problems.

## Development

See the [development guide](./docs/development.md) to learn how to run and develop this app locally.
