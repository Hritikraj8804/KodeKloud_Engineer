# Instruction

You are tasked with modifying an ARM template for deploying a Virtual Network.

The existing template is located at:

```bash
/root/arm-templates/vnet-deployment-template.json
```

Make the following changes:

| Property             | New Value                  |
| -------------------- | -------------------------- |
| Virtual Network Name | `arm-vnet-nautilus`        |
| Tag `displayName`    | `arm-vnet-nautilus`        |
| Address Prefix       | `192.168.0.0/16`           |
| New Tag              | `Environment=KKE-nautilus` |

After updating the template, deploy it using Azure CLI.

Resource Group:

```text
kml_rg_main-5d241e99b7e8474e
```


# Solution

## **1. Navigate to the ARM Template Directory**

```bash
cd /root/arm-templates
```


## **2. Inspect the Existing Template**

```bash
cat vnet-deployment-template.json
```

<img width="926" height="547" alt="preconfig" src="https://github.com/user-attachments/assets/2422945c-d338-4116-89b3-375857153e89" />


or

```bash
vi vnet-deployment-template.json
```


## **3. Modify the ARM Template**

Look for the VNet resource section and update it.

### Change VNet Name

Find:

```json
"name": "some-vnet-name"
```

Replace with:

```json
"name": "arm-vnet-nautilus"
```


### Change displayName Tag

Find the tags block:

```json
"tags": {
    "displayName": "old-value"
}
```

Update to:

```json
"tags": {
    "displayName": "arm-vnet-nautilus",
    "Environment": "KKE-nautilus"
}
```


### Change Address Prefix

Find:

```json
"addressSpace": {
    "addressPrefixes": [
        "10.0.0.0/16"
    ]
}
```

Replace with:

```json
"addressSpace": {
    "addressPrefixes": [
        "192.168.0.0/16"
    ]
}
```


## **4. Save the File**

If using vi:

```text
Esc
:wq
```

<img width="828" height="562" alt="postconfig" src="https://github.com/user-attachments/assets/55d26367-95d1-4537-b436-fd72dc579e97" />


## **5. Validate the ARM Template (Optional)**

```bash
az deployment group validate \
  --resource-group kml_rg_main-5d241e99b7e8474e \
  --template-file /root/arm-templates/vnet-deployment-template.json
```

Expected output:

```text
Succeeded
```


## **6. Deploy the ARM Template**

```bash
az deployment group create \
  --resource-group kml_rg_main-5d241e99b7e8474e \
  --template-file /root/arm-templates/vnet-deployment-template.json
```

<img width="1022" height="762" alt="deploy" src="https://github.com/user-attachments/assets/a686ff26-f614-4e2a-83cb-545996f77288" />


## **7. Verify the Deployment**

Check deployment status:

```bash
az deployment group list \
  --resource-group kml_rg_main-5d241e99b7e8474e \
  --output table
```


## **8. Verify the VNet**

```bash
az network vnet show \
  --resource-group kml_rg_main-5d241e99b7e8474e \
  --name arm-vnet-nautilus \
  --output json
```

Verify:

### Name

```json
"name": "arm-vnet-nautilus"
```

### Address Prefix

```json
"addressPrefixes": [
  "192.168.0.0/16"
]
```

### Tags

```json
"tags": {
  "displayName": "arm-vnet-nautilus",
  "Environment": "KKE-nautilus"
}
```


# Verification Checklist
To verify the deployment:
```bash
az network vnet show \
  --resource-group kml_rg_main-5d241e99b7e8474e \
  --name arm-vnet-nautilus \
  --query "{name:name,addressPrefixes:addressSpace.addressPrefixes,tags:tags}"
```

<img width="713" height="307" alt="verify" src="https://github.com/user-attachments/assets/d9244246-2136-4518-a4cf-ebfbe0da69fc" />


## Fun Message

*"Your ARM template has been upgraded and deployed successfully into Azure's networking universe ☁️🌐🚀"*
