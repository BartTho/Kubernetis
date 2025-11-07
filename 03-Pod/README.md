BRON: https://github.com/BretFisher/podspec
Alle Info : https://www.youtube.com/watch?v=4CzG4Uqd9jM

# Kubernetes Pod-specificatie: goede standaardinstellingen

- De Pod-specificatie voor je apps kan een van de complexere onderdelen van je Kubernetes-manifestontwerp zijn en vereist veel ingeschakelde functies om een ​​veilige en redelijk veilige standaard te zijn.

- Deze repository met één bestand is bedoeld als startpunt voor je Pod-specificaties en als aanvulling op implementaties, DaemonSets, StatefulSets, initContainers, enz.

# Aanvullende factoren en suggesties die van invloed zijn op pod-specificaties

- Voor spec.containers.resources is het goed om te bekijken hoe Kubernetes Quality of Service (QoS) werkt, aangezien dit van invloed is op het moment dat uw pod van een node wordt verwijderd wanneer de resources op zijn. Als uw limieten bijvoorbeeld niet overeenkomen met uw verzoeken, ontvangt uw pod alleen een QoS-klasse van Burstable in plaats van het hoogste niveau van Guaranteed.
- U kunt runAsUser/runAsGroup verwijderen als u een Dockerfile gebruikt die de gebruiker/groep instelt op niet-root (of ko of buildpacks, bedankt @e_k_anderson), maar sommige teams vereisen nog steeds dat deze waarden hardgecodeerd zijn in het manifest (of in de admission controller) om deze aan de serverzijde af te dwingen.
- Als runAsNonRoot true is (zoals het hoort), kunt u de foutmelding CreateContainerConfigError krijgen: Fout: container heeft runAsNonRoot en image heeft een niet-numerieke gebruiker (gebruikersnaam), kan niet verifiëren dat de gebruiker niet-root is. als je Dockerfile USER geen ID is. Kubernetes wil het als een ID (geen gebruiksvriendelijke gebruikersnaam zoals node) om ervoor te zorgen dat het niet zomaar een gebruiker is die aan UID 0 (root) wordt gekoppeld. Ik denk dat dit kan worden voorkomen door de gebruiker ook hard te coderen in het manifest (runAsUser), maar ik heb dat niet getest.
- Als je meer dan ~1000 services in een namespace hebt, kun je pod.spec.enableServiceLinks: false instellen om kleine vertragingen bij het opstarten van de container en TCP-return-trip te voorkomen, bedankt @e_k_anderson.
- Je kunt pod.spec.containers.imagePullPolicy waarschijnlijk vermijden, omdat de standaardinstellingen slim zijn en meestal de juiste werking hebben.
- pod.spec.containers.securityContext.readOnlyRootFilesystem is een goed idee als dat mogelijk is, maar werkt meestal niet direct met monolieten en traditionele apps. Het kan per situatie verschillen.