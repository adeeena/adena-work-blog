---
title: 'C# - Verify a method is called or not in Unit Test'
media_order: marcel-strauss-3yL5YqElghU-unsplash.jpg
date: '20:37 13-03-2021'
taxonomy:
    category:
        - 'C#'
        - 'Unit testing'
        - Moq
    tag:
        - 'C#'
        - 'Unit testing'
        - Moq
hide_git_sync_repo_link: false
hero_classes: text-light
hero_image: marcel-strauss-3yL5YqElghU-unsplash.jpg
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

===

I have a unit test I am checking whether a method is called once or not so I attempted this way:-

This is my Mock of `ILicenseManagerService` and I am passing its object through constructor.

>         public Mock<ILicenseManagerService> LicenseManagerService { get { return SetLicenseManagerServiceMock(); } }
>     
>             private Mock<ILicenseManagerService> SetLicenseManagerServiceMock()
>             {
>                 var licencemangerservicemock = new Mock<ILicenseManagerService>();
>                 licencemangerservicemock.Setup(m => m.LoadProductLicenses()).Returns(ListOfProductLicense).Verifiable();
>     
>                 return licencemangerservicemock;
>             }
>     
>             public static async Task<IEnumerable<IProductLicense>> ListOfProductLicense()
>             {
>                 var datetimeoffset = new DateTimeOffset(DateTime.Now);
>     
>                 var lst = new List<IProductLicense>
>                 {
>                     GetProductLicense(true, datetimeoffset, false, "1"),
>                     GetProductLicense(true, datetimeoffset, false, "2"),
>                     GetProductLicense(true, datetimeoffset, true, "3")
>                 };
>     
>                 return lst;
>             }

I am using this mock object to set `_licenseManagerService` and calling the `LoadProductLicenses()` in method under test. like this. licences are coming fine.

    var licenses = (await _licenseManagerService.LoadProductLicenses()).ToList();
My attempt for verify the call to this method -

     LicenseManagerService.Verify(m => m.LoadProductLicenses(),Times.Once);
But when I run my unit test, an exception coming that say method is not invoked at all. Where I am doing wrong ?

EDIT I am invoking the same mock here is my unit test.

>         [TestMethod]
>             [TestCategory("InApp-InAppStore")]
>             public async Task return_products_from_web_when_cache_is_empty()
>             {
>                 // this class basically for setting up external dependencies
>                 // Like - LicenceManagerService in context, i am using this mock only no new mock.
>                 var inAppMock = new InAppMock ();                  
>     
>     
>                 // object of Class under test- I used static method for passing external         
>                 //services for easy to change 
>                 var inAppStore = StaticMethods.GetInAppStore(inAppMock);
>     
>                 // method is called in this method
>                 var result = await inAppStore.LoadProductsFromCacheOrWeb();
>     
>                 // like you can see using the same inAppMock object and same LicenseManagerService
>                 inAppMock.LicenseManagerService.Verify(m => m.LoadProductLicenses(),Times.Once);   
>             }
>             

### Solution

>     LicenseManagerService.Verify(m => m.LoadProductLicenses(),Times.Once);

By calling the `LicenseManagerService` property, you're creating a new mock object. Naturally, no invocations have ever been performed on this instance.

You should change this property's implementation to return the same instance every time it is called.
    
### Source
[StackOverflow - https://stackoverflow.com/questions/24402945/verify-a-method-is-called-or-not-in-unit-test](https://stackoverflow.com/questions/24402945/verify-a-method-is-called-or-not-in-unit-test)
    
[Archive - https://archive.ph/XcMVw](https://archive.ph/XcMVw)