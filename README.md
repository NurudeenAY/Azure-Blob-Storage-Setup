# Azure Blob Storage Project: Secure Setup with Anonymous Access

## 1. Project Overview

This project demonstrates the creation of a secure Azure Blob Storage account with controlled anonymous access. The primary goals were to:

* Create a storage account utilizing locally redundant storage (LRS).
* Configure blob containers and successfully upload files.
* Adjust access levels to enable anonymous read access specifically for blobs.

**Tool used:** Microsoft Azure Portal

## 2. Objectives

* [✓] Create a storage account with the capability for anonymous access enabled at the container level.
* [✓] Upload a sample file (e.g., an image) to a blob container.
* [✓] Validate the configured access permissions by successfully retrieving the blob via its URL.
* [✓] Clean up all created Azure resources post-validation (simulated via sandbox environment).

## 3. Step-by-Step Implementation

### 3.1 Create an Azure Storage Account

1.  **Initial Step:** Signed in to the Azure Portal and navigated to `Create a Resource > Storage Account`.
   
    <img width="1349" alt="Bildschirmfoto 2025-05-04 um 19 17 12" src="https://github.com/user-attachments/assets/834ded04-1403-4135-b22a-98d4b7a87242" />

2.  **Key Configuration Settings:**
    * **Subscription:** Concierge Subscription (sandbox environment).
    * **Resource Group:** `learn-xxxx` (auto-generated for the sandbox environment).
    * **Storage Account Name:** Provided a globally unique name.
    * **Region:** Selected an appropriate Azure region (e.g., (US) West US).
    * **Performance:** Standard.
    * **Redundancy:** Locally-redundant storage (LRS).
    * **Advanced Tab - Anonymous Access:** Ensured "Allow enabling anonymous access on individual containers" was **checked**.
      

  <img width="1349" alt="Bildschirmfoto 2025-05-04 um 19 17 34" src="https://github.com/user-attachments/assets/a9ee7735-3990-45c3-a699-19fdfe3ec6b3" />


 <img width="1349" alt="Bildschirmfoto 2025-05-04 um 19 18 53" src="https://github.com/user-attachments/assets/0ffc668b-dd46-49ed-934d-c3234cced260" />


4.  **Review and Create:** Reviewed settings and deployed the storage account. Waited for successful deployment confirmation.


    <img width="1349" alt="Bildschirmfoto 2025-05-04 um 19 20 18" src="https://github.com/user-attachments/assets/3aa5edbd-90e3-4fa9-af85-c2787f502b01" />


### 3.2 Create a Blob Container and Upload Files

1.  **Navigate to Containers:** Within the newly created storage account, went to `Data storage > Containers`.
2.  **Create New Container:**
    * Clicked on `+ Container`.
    * **Name:** Provided a name for the container (e.g., `cont-ny`).
    * **Anonymous access level:** Initially set to `Private (no anonymous access)`.
    

 <img width="1349" alt="Bildschirmfoto 2025-05-04 um 19 21 00" src="https://github.com/user-attachments/assets/13b453d4-0313-4779-9b7d-674ed361762d" />


   * Clicked `Create`.
      

  <img width="1349" alt="Bildschirmfoto 2025-05-04 um 19 23 50" src="https://github.com/user-attachments/assets/dd4ad349-2330-4bfe-8f91-f6a0284f286b" />


4.  **Upload File (Blob):**
    * Selected the created container.
    * Clicked `Upload`.
    * Browsed for a sample image file on the local machine and uploaded it.
      
   
   <img width="1349" alt="Bildschirmfoto 2025-05-04 um 19 25 59" src="https://github.com/user-attachments/assets/77311404-6efb-4c07-8ee5-0ce31148b868" />


### 3.3 Adjust Blob Access Level and Validate

1.  **Initial Access Test (Private):**
    * Selected the uploaded blob.
    * Copied its URL from the properties.
    * Pasted the URL into a new browser tab.
    * **Result:** Received a `ResourceNotFound` error, as expected due to the private access level.
    

     <img width="1118" alt="Bildschirmfoto 2025-05-04 um 19 27 38" src="https://github.com/user-attachments/assets/8321034d-6e26-4dee-bd88-af154225e0f4" />


3.  **Change Access Level:**
    * Navigated back to the container settings.
    * Selected `Change access level`.
    * Changed `Anonymous access level` from `Private (no anonymous access)` to `Blob (anonymous read access for blobs only)`.
    

    <img width="1347" alt="Bildschirmfoto 2025-05-04 um 19 28 53" src="https://github.com/user-attachments/assets/0617a48b-db82-4244-bb1c-7e5eff91fbe3" />


    * Clicked `OK`.
5.  **Validation:**
    * Refreshed the browser tab where the blob URL was previously opened (or pasted it again).
    * **Result:** The image was successfully displayed, confirming anonymous read access for the blob.
    

     ![hacker-criminal-with-a-laptop-vector](https://github.com/user-attachments/assets/70e9709e-dcf7-47a8-8ab5-45afc1e43e1c)


## 4. Challenges & Solutions

1.  **Challenge:** Initial attempt to access the blob URL resulted in a `404 Resource Not Found` error.
    * **Solution:** Correctly identified that the container's anonymous access level was still set to `Private`. Adjusted it to `Blob (anonymous read access for blobs only)` to resolve the issue.
2.  **Challenge:** Ensuring secure transfer for REST API operations.
    * **Solution:** Verified and ensured that `Require secure transfer for REST API operations` was enabled in the storage account's `Advanced` settings during creation (or could be confirmed/modified in Configuration settings post-creation).

## 5. Results & Validation

* **Validation:** Successfully accessed the uploaded blob via its URL after changing the container's access level to allow anonymous read access for blobs. [cite: 23] This confirmed the correct configuration.
* **Performance Tier:** Utilized the `Standard` performance tier with `Locally-redundant storage (LRS)` for a balance of cost-effectiveness and data durability for this exercise.
* **Security Measures:**
    * Enabled `Secure transfer required` for REST API operations.
    * Restricted anonymous access specifically to `blobs only`, preventing anonymous listing of container contents.

## 6. Architecture Diagram

The following diagram illustrates the high-level architecture of the setup:

```mermaid
graph TD
    A[User] -- Interacts with --> B{Azure Portal};
    B -- Creates/Manages --> C[Resource Group];
    C -- Contains --> D[Storage Account];
    D -- Contains --> E[Container];
    E -- Contains --> F[Blob/File];
    B -- Uploads file to --> E;
    B -- Configures access level for --> E;
    G[Public Internet/Browser] -- Accesses via URL --> F;
