# Pact Provider Test - playground-provider-quarkus

Dieses Projekt enthält einen Pact Provider Test für den `playground-provider-quarkus` Service.

## Überblick

Der Pact Provider Test verifiziert, dass Ihr Provider die Verträge (Pacts) erfüllt, die von Consumern definiert wurden.

## Test-Setup

### Lokale Pact-Dateien

Standardmäßig lädt der Test Pact-Dateien aus dem Classpath unter `src/test/resources/pacts/`:

```java
@PactFolder("pacts")
```

**Wichtig**: Platzieren Sie Ihre Pact-JSON-Dateien in `src/test/resources/pacts/` (nicht im Projektroot), damit sie vom Test gefunden werden.

### Pact Broker

Alternativ können Sie einen Pact Broker verwenden. Kommentieren Sie dazu die `@PactFolder` Annotation aus und aktivieren Sie die `@PactBroker` Annotation:

```java
// @PactFolder("pacts")
@PactBroker(url = "${PACT_BROKER_URL:http://localhost:9292}")
```

Setzen Sie dann die Umgebungsvariable `PACT_BROKER_URL` oder konfigurieren Sie sie in Ihrer `application.properties`.

## Provider States

Der Test unterstützt verschiedene Provider States durch `@State` annotierte Methoden:

```java
@State("demo endpoint exists")
public void demoEndpointExists() {
    // Setup für diesen spezifischen State
    System.out.println("Provider state: Demo endpoint is available");
}
```

## Test ausführen

### Alle Tests
```bash
mvn test
```

### Nur Pact Provider Tests
```bash
mvn test -Dtest=PactProviderTest
```

### Mit spezifischem Pact Broker
```bash
mvn test -DPACT_BROKER_URL=http://your-broker-url:9292
```

## Konfiguration

Die wichtigsten Konfigurationsoptionen:

- **Provider Name**: `playground-provider-quarkus` (definiert in `@Provider`)
- **Test Port**: Der Test verwendet automatisch den Quarkus Test Port
- **Pact-Verzeichnis**: `pacts/` (konfigurierbar über `@PactFolder`)

## Struktur

```
src/
├── main/java/de/tollcollect/
│   └── ExampleResource.java        # Ihr Provider Service
├── test/java/de/tollcollect/
│   └── PactProviderTest.java       # Pact Provider Test
└── test/resources/pacts/
    └── playground-consumer-quarkus-playground-provider-quarkus.json  # Pact-Datei
pacts/
└── playground-consumer-quarkus-playground-provider-quarkus.json  # Optional: Backup
```

## Troubleshooting

### "MissingStateChangeMethod" Fehler
Wenn Sie einen Fehler wie `Did not find a test class method annotated with @State("...")` erhalten, fügen Sie eine entsprechende `@State` Methode hinzu.

### Pact-Dateien nicht gefunden
Stellen Sie sicher, dass die Pact-Dateien im `src/test/resources/pacts/` Verzeichnis liegen (nicht im Projektroot) oder der Pact Broker erreichbar ist.

### Port-Konflikte
Der Test verwendet automatisch den Quarkus Test Port. Stellen Sie sicher, dass keine anderen Services auf diesem Port laufen.
