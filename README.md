# silence-notifier
Varsle teknisk om stillhet på streamen. Erstattet vår 2019 av https://github.com/Studentmediene/silence-notifier-teams grunnet migrering fra Slack til Microsoft Teams.

## Oppsett

1. Klon kodelageret: `git clone ...`
2. Kopier `rtmbot.conf.template` og kall den `rtmbot.conf`
3. Lag en ny fil kalt `settings.yaml`
4. Rediger `rtmbot.conf`, spesifikt legg inn Slack-nøkkel og sti til loggfil
5. Rediger `settings.yaml`, og redefiner `channel`, `liquidsoap_script` og `rr_api`. Redefiner også
   andre innstillinger du føler for å endre fra `settings_default.yaml`.
6. Lag et virtualenv: `virtualenv -p python3 venv` eller noe liknende
7. Tre inn i det virtuelle miljøet: `. venv/bin/activate`
8. Installer nødvendige pakker: `pip install -r requirements.txt`
9. Kopier `silence-notifier.service` inn i `/etc/systemd/system` og rediger
   filstien i den så den er riktig (`WorkingDirectory=...`).
10. Kjør `sudo visudo -f /etc/sudoers.d/silence-notifier-sudoers` og legg inn
    innholdet fra fila `silence-notifier-sudoers` her i kodelageret.
11. Lag bruker for `silence-notifier`: `sudo adduser silence-notifier --system --group`
11. Lag mappa `logs`, og gi `silence-notifier`-brukeren skriverettigheter til den:
    `sudo mkdir logs; sudo chown silence-notifier: logs`

## Kjøring

Skriptet kjøres med SystemD:

```sh
sudo --non-interactive /bin/systemctl start silence-notifier
```

og stoppes også med SystemD:

```sh
sudo --non-interactive /bin/systemctl stop silence-notifier
```

Disse kommandoene setter du til å kjøres i `transition`-funksjoner i fallback i
LiquidSoap. Du må bruke den absolutte filstien som ovenfor for at sudo-reglene
skal gjelde (siden hvem som helst kan lage et skript som heter systemctl).
