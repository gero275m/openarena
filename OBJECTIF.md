<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

## Objectif

Créer deux containers Docker distincts hébergeant chacun un serveur OpenArena, l’un configuré pour le mode Capture the Flag (CTF), l’autre pour le mode Deathmatch (DM), avec gestion des cartes (maps) de base et additionnelles, ajout/retrait automatique de bots, et système de votes (changement de map, kick, etc.).

---

## 1. Ressources nécessaires

- **Images de base** : Utiliser l’image officielle ou packagée d’OpenArena Server (paquet `openarena-server`)[^2][^1].
- **Cartes additionnelles** : Fichiers `.pk3` à placer dans le dossier `baseoa` du serveur[^7].
- **Fichiers de configuration** : `server.cfg` pour chaque mode de jeu, scripts de démarrage personnalisés.
- **Docker** : Dockerfile(s), docker-compose (facultatif pour le local), ou manifestes Kubernetes pour l’orchestration à plus grande échelle[^9].
- **Documentation OpenArena** : Wiki Ubuntu-fr pour les commandes et la console[^1][^7].

---

## 2. Étapes détaillées

### **A. Préparer les fichiers de configuration**

- Créez un dossier par mode de jeu : `ctf` et `dm`.
- Pour chaque mode, créez un fichier `server.cfg` avec les paramètres spécifiques :
    - `g_gametype 4` pour CTF, `g_gametype 0` pour Deathmatch[^4].
    - Configuration des votes :

```
set g_allowVote "1"
set g_voteFlags "12345678"  # Adapter selon les votes autorisés (kick, map, etc.)
```

    - Ajout de bots automatique (exemple pour 6 joueurs) :

```
set bot_minplayers "6"
set bot_nochat "1"
```

    - Liste des maps :

```
set sv_maplist "map1 map2 map3"
set g_maplist "map1 map2 map3"
```

    - Référencez la documentation OpenArena pour toutes les variables[^1][^7].


### **B. Préparer les cartes additionnelles**

- Téléchargez les fichiers `.pk3` des maps souhaitées.
- Placez-les dans un dossier `baseoa` qui sera monté dans le container[^7].


### **C. Dockerfile type**

```dockerfile
FROM debian:stable-slim

RUN apt-get update && \
    apt-get install -y openarena-server && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /srv/openarena

# Copie des configs et des maps
COPY baseoa /srv/openarena/baseoa
COPY server.cfg /srv/openarena/baseoa/server.cfg

EXPOSE 27960/udp

ENTRYPOINT ["openarena-server", "+set", "fs_homepath", "/srv/openarena", "+exec", "baseoa/server.cfg"]
```

- Adaptez le chemin du `server.cfg` et des maps selon le mode de jeu.


### **D. Structure du projet**

```
openarena-ctf/
  |- Dockerfile
  |- baseoa/
      |- map1.pk3
      |- map2.pk3
      |- ...
      |- server.cfg
openarena-dm/
  |- Dockerfile
  |- baseoa/
      |- map1.pk3
      |- map2.pk3
      |- ...
      |- server.cfg
```


### **E. Lancement des containers**

- Build :

```
docker build -t openarena-ctf ./openarena-ctf
docker build -t openarena-dm ./openarena-dm
```

- Run :

```
docker run -d --name openarena-ctf -p 27960:27960/udp openarena-ctf
docker run -d --name openarena-dm -p 27961:27960/udp openarena-dm
```


### **F. Orchestration et automatisation**

- Pour la production, utilisez Kubernetes ou Docker Swarm pour la gestion du scaling, du restart, etc.[^9].
- Exemple de manifest Kubernetes ou docker-compose pour le développement local.


### **G. Ajout/retrait automatique de bots**

- Géré via `bot_minplayers` dans le `server.cfg`[^4].
- Pour plus de contrôle, utilisez des scripts shell ou des commandes RCON pour ajuster dynamiquement le nombre de bots.


### **H. Système de votes**

- Activez les votes via `g_allowVote` et `g_voteFlags` dans le `server.cfg`[^4].
- Les joueurs pourront voter pour changer de map, kicker un joueur, etc.

---

## 3. Liens et documentation utiles

- [Wiki Ubuntu-fr OpenArena](http://doc.ubuntu-fr.org/openarena)[^1]
- [Forum Ubuntu-fr sur les maps et la console](https://forum.ubuntu-fr.org/viewtopic.php?id=305731)[^7]
- [Paquet Debian openarena-server](https://packages.debian.org/fr/sid/games/openarena-server)[^2]
- [Guide général sur la conteneurisation et l'orchestration](https://blog.stephane-robert.info/docs/conteneurisation/)[^9]

---

## 4. Conseils et bonnes pratiques

- Versionnez vos fichiers de configuration et scripts dans un dépôt Git.
- Séparez bien les volumes Docker pour les maps et configs afin de faciliter les mises à jour.
- Pour l’ajout de nouvelles maps, il suffit de déposer les `.pk3` dans le volume monté `baseoa` et de relancer le container si besoin.
- Testez localement avec Docker Compose avant de déployer sur un orchestrateur.

---

## 5. Exemple de fichier infrastructure as code (docker-compose)

```yaml
version: '3'
services:
  openarena-ctf:
    build: ./openarena-ctf
    ports:
      - "27960:27960/udp"
    volumes:
      - ./openarena-ctf/baseoa:/srv/openarena/baseoa
  openarena-dm:
    build: ./openarena-dm
    ports:
      - "27961:27960/udp"
    volumes:
      - ./openarena-dm/baseoa:/srv/openarena/baseoa
```


---

En suivant cette démarche, tu pourras déployer facilement deux serveurs OpenArena conteneurisés, chacun spécialisé dans son mode de jeu, avec gestion avancée des maps, bots, et votes, tout en gardant une infrastructure as code maintenable et évolutive[^1][^4][^7][^9].

<div style="text-align: center">⁂</div>

[^1]: http://doc.ubuntu-fr.org/openarena

[^2]: https://packages.debian.org/fr/sid/games/openarena-server

[^3]: https://camptocamp.com/fr/geoserver

[^4]: https://www.jeuxonline.info/jeu/Open_Arena

[^5]: https://camptocamp.com/fr/mapserver

[^6]: https://mindsers.blog/fr/post/deployer-simplement-conteneur-docker-production/

[^7]: https://forum.ubuntu-fr.org/viewtopic.php?id=305731

[^8]: https://learn.microsoft.com/fr-fr/azure/devops/pipelines/agents/docker?view=azure-devops

[^9]: https://blog.stephane-robert.info/docs/conteneurisation/

