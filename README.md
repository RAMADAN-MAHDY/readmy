# API Endpoints

Base URL: https://api.taklifa.com

## AuthService

- **POST** /api/auth/register
- **POST** /api/auth/login
- **POST** /api/auth/logout
- **POST** /api/auth/verify/email/verify
- **POST** /api/auth/verify/email/send-verification
- **POST** /api/auth/verify/phone-number/verify
- **POST** /api/auth/verify/phone-number/send-verification
- **POST** /api/auth/reset-password/request
- **POST** /api/auth/reset-password/check
- **POST** /api/auth/reset-password
- **POST** /api/auth/check-email-exist

## CartService

- **GET** /api/cart/{code}
- **POST** /api/cart/{code}/items
- **DELETE** /api/cart/{code}/items

## CompaniesService

- **GET** /api/companies
- **GET** /api/companies/{company}

## CompanyAdminService

- **GET** /api/admin/companies
- **POST** /api/admin/companies
- **GET** /api/admin/companies/{company}
- **PUT** /api/admin/companies/{company}
- **DELETE** /api/admin/companies/{company}
- **POST** /api/admin/companies/{company}/change-active-company

## CompanyBranchAdminService

- **GET** /api/admin/companies/{company}/branches
- **POST** /api/admin/companies/{company}/branches
- **GET** /api/admin/companies/{company}/branches/{companyBranch}
- **PUT** /api/admin/companies/{company}/branches/{companyBranch}
- **DELETE** /api/admin/companies/{company}/branches/{companyBranch}

## CompanyBranchService

- **GET** /api/companies/{company}/branches
- **GET** /api/companies/{company}/branches/{companyBranch}

## CompanyInvitationsService

- **GET** /api/admin/companies/{company}/invitation
- **POST** /api/admin/companies/{company}/invitation
- **GET** /api/admin/companies/{company}/invitation/{companyInvitation}
- **PUT** /api/admin/companies/{company}/invitation/{companyInvitation}
- **DELETE** /api/admin/companies/{company}/invitation/{companyInvitation}

## CompanyMemberInvitationService

- **GET** /api/company-invitation/{invitationCode}
- **POST** /api/company-invitation/{invitationCode}
- **POST** /api/company-invitation/{invitationCode}/reject

## CompanyMembersService

- **DELETE** /api/admin/companies/{company}/members/{member}
- **GET** /api/companies/{company}/members
- **GET** /api/companies/{company}/members/{member}

## FaqsService

- **GET** /api/faqs

## GeographyService

- **GET** /api/geography/countries
- **GET** /api/geography/countries/{country}
- **GET** /api/geography/countries/dial-code/{dialCode}
- **GET** /api/geography/states
- **GET** /api/geography/cities
- **GET** /api/geography/currencies

## LocationService

- **GET** /api/locations/{location}
- **PUT** /api/locations/{location}
- **POST** /api/locations
- **POST** /api/locations/live-location/create

## MediaService

- **DELETE** /api/media/uploads

## NotificationService

- **GET** /api/notifications
- **POST** /api/notifications/mark-all-as-read
- **GET** /api/notifications/unread-count
- **POST** /api/notifications/expo-token

## ProductCategoriesService

- **GET** /api/product-categories/parents
- **GET** /api/product-categories/{categoryId}
- **GET** /api/product-categories/{mainCategoryId}/sub-categories

## ProductsService

- **GET** /api/products
- **POST** /api/products
- **GET** /api/products/{product}
- **PUT** /api/products/{product}
- **DELETE** /api/products/{product}
- **POST** /api/products/{product}/publish
- **POST** /api/products/ai/batch-create
- **POST** /api/products/ai/batch-create/images
- **POST** /api/products/ai/update/images

## RatingService

- **GET** /api/rating-types
- **GET** /api/ratings/{id}
- **POST** /api/rating

## ServiceService

- **GET** /api/services
- **POST** /api/services
- **GET** /api/services/{service}
- **PUT** /api/services/{service}
- **DELETE** /api/services/{service}
- **GET** /api/service-categories

## SupportService

- **GET** /api/support/categories
- **POST** /api/support
- **GET** /api/support/report-reasons
- **POST** /api/support/reports

## UserService

- **GET** /api/auth/user
- **PUT** /api/auth/user
- **POST** /api/auth/user/update-password
- **POST** /api/auth/user/update-email
- **POST** /api/auth/user/update-phone-number
- **POST** /api/auth/user/update-location
- **POST** /api/auth/user/change-active-role
- **DELETE** /api/auth/user/delete-account
- **POST** /api/auth/user/enable-customer-role

## UsersService

- **GET** /api/users
- **GET** /api/users/{user}
