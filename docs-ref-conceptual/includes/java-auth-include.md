<span data-ttu-id="c95d7-101">Creare un [file di autenticazione](../java-sdk-azure-authenticate.md#mgmt-file) ed esportare una variabile di ambiente `AZURE_AUTH_LOCATION` nella riga di comando con il percorso completo del file.</span><span class="sxs-lookup"><span data-stu-id="c95d7-101">Create an [authentication file](../java-sdk-azure-authenticate.md#mgmt-file) and export an environment variable `AZURE_AUTH_LOCATION` on the command line with the full path to the file.</span></span>

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azure.auth
```

<span data-ttu-id="c95d7-102">Il file di autenticazione consente di configurare l'oggetto `Azure` del punto di ingresso usato dalle librerie di gestione per definire, creare e configurare le risorse di Azure.</span><span class="sxs-lookup"><span data-stu-id="c95d7-102">The authentication file is used to configure the entry point `Azure` object used by the management libraries to define, create, and configure Azure resources.</span></span>

```java
// pull in the location of the security file from the environment 
final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));

Azure azure = Azure
        .configure()
        .withLogLevel(LogLevel.NONE)
        .authenticate(credFile)
        .withDefaultSubscription();
```

<span data-ttu-id="c95d7-103">[Altre informazioni](../java-sdk-azure-authenticate.md#mgmt-auth) sulle opzioni di autenticazione quando si usano le librerie di gestione di Azure per Java.</span><span class="sxs-lookup"><span data-stu-id="c95d7-103">[Learn more](../java-sdk-azure-authenticate.md#mgmt-auth) about authentication options when using the Azure management libraries for Java.</span></span>