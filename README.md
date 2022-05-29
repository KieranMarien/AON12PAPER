# AON12 - PWA 
## Simpele ToDo PWA 


Github repository voor de researchpaper van AON 12 van de PXL. Bestaat uit twee delen, een frontend AON12PAPER en een backend TODOER. 

## Frontend

Kleine todo applicatie waar taken samen met een datum en tijd ingegeven kunnen worden. Men krijgt een toaster melding op de opgegeven tijd, als de applicatie geïnstalleerd is en er op de notificatie wordt gedrukt dan apent de applicatie. 
### Opstarten 
```sh
npm install
npm run build
http-server dist -p 8080
```

Wordt best op chrome opgestart, hier kan men de PWA installeren. Vergeet niet in de computer instellingen notificaties van chrome toestaan.



## Backend

Is een kleine API die op de aangegeven tijd een melding naar de service worker van de frontend stuurt. De service worker kan dan de notificatie laten zien.

### Opstarten 
ToDoBackEndApplication.java runnen.

## Sources 

https://github.com/marcushellberg/hilla-push-notifications
https://github.com/gauntface/simple-push-demo



