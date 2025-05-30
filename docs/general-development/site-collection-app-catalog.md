---
title: Use the site collection app catalog
description: Using site collection app catalogs, SharePoint tenant administrators can decentralize the management and scope the deployment of SharePoint add-ins and SharePoint Framework solutions to specific sites.
ms.date: 04/23/2025
ms.assetid: fdf7ecb1-9951-475b-b058-3285fba77b68
ms.localizationpriority: high
---

# Use the site collection app catalog

_**Applies to:** Office 365_

Using site collection app catalogs, SharePoint tenant administrators can decentralize the management and scope the deployment of SharePoint add-ins and SharePoint Framework solutions to specific sites.

## Why site collection app catalogs

Previously, all add-ins and SharePoint Framework solutions had to be managed centrally in the tenant app catalog. While tenant administrators could delegate access to other people in the organization, a deployed package was visible on all site collections. SharePoint offered no supported way of deploying add-ins and SharePoint Framework solutions only to specific sites.

With the introduction of site collection app catalogs, tenant administrators can enable the app catalog on specific sites. Once enabled, site collection administrators can deploy SharePoint add-ins and SharePoint Framework solutions that will be available only in that particular site collection.

The following schema illustrates using site collection app catalogs:

![Diagram illustrating the concept of site collection app catalog](../images/site-collection-app-catalog-diagram.png)

In your Office 365 tenant, you have a tenant app catalog. Solutions deployed to this app catalog can be installed in any site collection in the tenant. Tenant administrators can choose to enable site collection app catalogs on specific site collections. Solutions deployed to the site collection app catalogs can only be installed in that particular site collection.

## Supported capabilities

### Support for both SharePoint add-ins and SharePoint Framework packages

In site collection app catalogs, just as in tenant app catalogs, you can deploy both SharePoint add-ins and SharePoint Framework solutions (.sppkg).

### Including assets in solution packages

SharePoint Framework solution packages that contain assets can be deployed to site collection app catalogs. Included assets will be deployed to a preconfigured document library in the same site collection as where the site collection app catalog is located. If the Office 365 Public CDN is configured, assets will be served from the CDN. Otherwise, assets will be served directly from the document library.

### Tenant-scoped deployment

When deploying SharePoint Framework solutions that support tenant-wide deployment to a site collection app catalog, you will be prompted if you want to make this solution available to all sites in the organization. Despite the wording, if you check this box, the solution will be available immediately **only in the same site collection as where the app catalog is**. Other site collections in your organization will not be able to use the solution. If you don't check this option, you will have to explicitly install the solution on your site before you will be able to use it.

## Current limitations

## Configure and manage site collection app catalogs

You can configure and manage site collection app catalogs using the SharePoint Online Management Shell.

> [!NOTE]
> Before you can manage site collection app catalogs in your tenant, ensure that you have installed [SharePoint Online Management Shell](https://www.microsoft.com/download/details.aspx?id=35588) from November 2017 or newer.

Alternatively, you can use the [CLI for Microsoft 365](https://sharepoint.github.io/office365-cli?utm_source=msft_docs&utm_medium=page&utm_campaign=Use+the+site+collection+app+catalog) to manage your SharePoint site collection app catalogs. The CLI for Microsoft 365 is a cross-platform command line interface that can be used on any platform, including Windows, macOS, and Linux. Using [PnP PowerShell](/powershell/sharepoint/sharepoint-pnp/sharepoint-pnp-cmdlets) to [create the app catalog](https://pnp.github.io/powershell/cmdlets/Add-PnPSiteCollectionAppCatalog.html) or [remove the app catalog](https://pnp.github.io/powershell/cmdlets/Remove-PnPSiteCollectionAppCatalog.html) is also an option when using Windows.

[!INCLUDE [pnp-powershell](../../includes/snippets/open-source/pnp-powershell.md)]

[!INCLUDE [pnp-o365cli](../../includes/snippets/open-source/pnp-o365cli.md)]

### Create a site collection app catalog

> [!NOTE]
> Before running the following script, connect to your SharePoint Online tenant using the `Connect-SPOService` cmdlet when using the SharePoint Online PowerShell. Also, ensure that you have a tenant app catalog created in your tenant (Multi-geo customers will need to create a tenant app catalog for each geo they wish to use a site collection app catalog). If you don't, the cmdlet will fail with the following error:
>
> ```text
> Cannot invoke method or retrieve property from null object. Object returned by the
> following call stack is null. "TenantAppCatalog
> RootWeb
> GetSiteByUrl
> new Microsoft.Online.SharePoint.TenantAdministration.Tenant()
> "
> ```
>
> Alternatively, if you are using the CLI for Microsoft 365, you must first connect to your Microsoft 365 tenant using the `m365 login` command. With PnP PowerShell you would use `Connect-PnPOnline -Url https://<tenant>-admin.sharepoint.com -Interactive -ClientId <your-client-id>` (using [a registered Azure AD (Entra ID) application](https://pnp.github.io/powershell/articles/registerapplication.html)) to set up the connection.

> [!CAUTION]
> The account used to create the App Catalog Site Collection must be a Site Collection Administrator on both the tenant-level App Catalog and the target Site Collection

To create a site collection app catalog, use the `Add-SPOSiteCollectionAppCatalog` cmdlet, passing the site collection where the app catalog should be created as the `-Site` parameter.

```powershell
Add-SPOSiteCollectionAppCatalog -Site https://contoso.sharepoint.com/sites/marketing
```

Alternatively, use PnP PowerShell to add the site app catalog functionality to your site after having connected to the SharePoint Online Admin site:

```powershell
Add-PnPSiteCollectionAppCatalog -site https://contoso.sharepoint.com/sites/marketing
```

Alternatively, use the `spo site appcatalog add` command if you are using the CLI for Microsoft 365:

```console
m365 spo site appcatalog add --siteUrl https://contoso.sharepoint.com/sites/marketing
```

After executing this script, the **Apps for SharePoint** library will be added to your site collection, where you will be able to deploy SharePoint add-ins and SharePoint Framework solutions.

### Disable the site collection app catalog

> [!NOTE]
> Before running the following script, connect to your SharePoint Online tenant using the `Connect-SPOService` cmdlet for the SharePoint Online PowerShell, `Connect-PnPOnline -Url https://<tenant>-admin.sharepoint.com -Interactive -ClientId <your-client-id>` for PnP PowerShell (using a [registered Azure AD (Entra ID) application](https://pnp.github.io/powershell/articles/registerapplication.html)), or m365 login` command for the CLI for Microsoft 365 to connect to your Microsoft 365 tenant.

To disable the site collection app catalog in your site collection, use the `Remove-SPOSiteCollectionAppCatalog` cmdle,t passing the site collection where the app catalog should be disabled as the `-Site` parameter. Alternatively, if you have your site collection's ID, you can use the `Remove-SPOSiteCollectionAppCatalogById` cmdlet instead.

> [!NOTE]
> Despite the naming, the `Remove-SPOSiteCollectionAppCatalog` and `Remove-SPOSiteCollectionAppCatalogById` cmdlets don't remove the site collection app catalog from the site collection. Instead, they disable it so that it's not possible to deploy or use any solutions deployed in it.

```powershell
Remove-SPOSiteCollectionAppCatalog -Site https://contoso.sharepoint.com/sites/marketing
```

Alternatively, use PnP PowerShell to remove the site app catalog functionality from your site after having connected to the SharePoint Online Admin site:

```powershell
Remove-PnPSiteCollectionAppCatalog -site https://contoso.sharepoint.com/sites/marketing
```

Alternatively, use the `spo site appcatalog remove` command if you are using the CLI for Microsoft 365

```console
m365 spo site appcatalog remove --url https://contoso.sharepoint.com/sites/marketing
```

After executing this script, the **Apps for SharePoint** library will still be visible in your site collection, but you will not be able to deploy or use any solutions deployed in it.

![Screenshot illustrating how the app catalog will disallow adding new apps after it has been removed](../images/site-collection-app-catalog-disabled.png)

## Considerations

### Governance

To list all site collections in the tenant that have the site collection app catalog enabled, use the URL `https://<tenant-app-catalog-URL>/Lists/SiteCollectionAppCatalogs/AllItems.aspx`.

### Security

Before deploying solutions to site collection app catalogs, site collection administrators should verify that these solutions meet organizational policies. Although solutions installed in site collection app catalogs can only be used in these particular site collections, they can potentially access resources from other sites in the tenant, so administrators should ensure that the solutions they are about to deploy work as intended.

## See also

- [Manage the Organizational App Catalog](/sharepoint/use-app-catalog)
- [CLI for Microsoft 365](https://sharepoint.github.io/office365-cli?utm_source=msft_docs&utm_medium=page&utm_campaign=Use+the+site+collection+app+catalog)
