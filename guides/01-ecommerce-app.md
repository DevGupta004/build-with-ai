# Build a Full E-Commerce App with Claude & Fable 5
### From Zero to Working Product — Step by Step

> **Who is this for?** Anyone who wants to build a real, feature-loaded e-commerce app without a full dev team. You give Claude the right prompts — it writes the code.

---

## What We're Building

| App | What It Is | Tech |
|---|---|---|
| **SwiftMart** | Customer mobile app (iOS + Android) | React Native 0.85 |
| **SwiftMartBackend** | REST API + Admin panel | Node.js 20 + Next.js 15 |

---

## Tech Stack

### Mobile App (SwiftMart)

```
Core
────────────────────────────────────────
react-native 0.85          → iOS + Android (NO Expo)
react 19                   → UI framework
typescript 5               → type safety

Navigation
────────────────────────────────────────
@react-navigation/native              → navigation container
@react-navigation/native-stack        → stack navigator
@react-navigation/bottom-tabs         → tab bar

State & Storage
────────────────────────────────────────
zustand 5                  → global state
react-native-mmkv          → fast persistent storage (token, cart, prefs)

Data Fetching
────────────────────────────────────────
@tanstack/react-query v5   → server state, caching, pagination
axios                      → HTTP client

UI & Animation
────────────────────────────────────────
react-native-reanimated    → smooth animations
react-native-gesture-handler → swipe, pan gestures
react-native-safe-area-context → notch/island safe areas
react-native-screens       → native screen performance
react-native-fast-image    → cached image loading
react-native-linear-gradient → gradient backgrounds
react-native-svg           → vector graphics
react-native-vector-icons  → icon library

Forms & Validation
────────────────────────────────────────
react-hook-form            → form state management
zod                        → schema validation

Internationalization
────────────────────────────────────────
i18next                    → translation engine
react-i18next              → React bindings
react-native-localize      → device locale detection (8 langs, Arabic RTL)

Device & Permissions
────────────────────────────────────────
react-native-device-info   → app version, device ID
react-native-permissions   → runtime permissions
react-native-geolocation-service → GPS for address auto-fill
react-native-image-picker  → camera / gallery picker
react-native-maps          → map view for address

Firebase
────────────────────────────────────────
@react-native-firebase/app       → Firebase core
@react-native-firebase/auth      → phone OTP authentication
@react-native-firebase/messaging → push notifications (FCM)

Other
────────────────────────────────────────
react-native-webview       → payment gateway WebView
@shopify/flash-list        → high-performance list (replaces FlatList)
react-native-webview       → in-app browser for payments
@react-native-community/netinfo → network status
```

### Backend API (SwiftMartBackend/server)

```
Node.js 20 LTS       → runtime
Express 4            → REST API framework
TypeScript 5 strict  → type safety
Prisma 6             → ORM (MySQL provider)
MySQL 8              → database
jsonwebtoken         → JWT auth (separate secrets: user + admin)
bcryptjs             → password hashing
multer               → file upload (images)
sharp                → image resize + optimization
dotenv               → environment config
zod                  → request validation
helmet               → HTTP security headers
cors                 → cross-origin requests
morgan               → request logging
firebase-admin       → send FCM push notifications
PM2                  → production process manager
```

### Admin Panel (SwiftMartBackend/admin)

```
Next.js 15 App Router → admin web UI (served same port as API)
TypeScript 5 strict   → type safety
Custom Express server → unified: Express API + Next.js on port 3000
Prisma 6              → same DB, server-side queries
JWT (admin scope)     → separate admin token
bcryptjs              → admin password hash
Inline SVG charts     → revenue + orders trend (no lib needed)
Custom CSS            → dark sidebar + white content (no Tailwind needed)
```

---

## Before You Start

### Install These First

```bash
# Node.js 20+ (nodejs.org)
node -v   # should show v20+

# Claude Code CLI
npm install -g @anthropic-ai/claude-code

# React Native CLI
npm install -g react-native-cli

# Watchman (Mac only)
brew install watchman

# Android Studio + Android SDK (for Android)
# Xcode (for iOS — Mac only)
```

### MySQL Setup
- Mac:   `brew install mysql && brew services start mysql`
- Windows: Download XAMPP → start MySQL
- Create DB: `CREATE DATABASE swiftmart_db;`

### Set Claude to Fable 5
In Claude Code — type `/model` and select **claude-fable-5**
Or add to your project `.claude/settings.json`:
```json
{ "model": "claude-fable-5" }
```

> Fable 5 writes 500+ line files in one shot without stopping mid-file.

---

## Part 1 — Mobile App (SwiftMart)

### Step 1 — Init React Native project first

```bash
npx react-native@latest init SwiftMart
cd SwiftMart
```

> **Why first?** RN CLI scaffolds android/ and ios/ native folders. Claude then fills in all the src/ code without overwriting native config.

### Step 2 — Open Claude Code inside the project

```bash
cd SwiftMart
claude
```

Or: open VS Code in the SwiftMart folder → use Claude Code extension sidebar.

### Step 3 — Paste this prompt

---

```
I have a freshly init'd React Native CLI project called SwiftMart (no Expo).
The android/ and ios/ folders already exist. Fill in all the source code.

Tech stack (already in package.json — do not change versions):
- react-native 0.85, react 19, typescript 5
- @react-navigation/native + native-stack + bottom-tabs
- zustand 5 + react-native-mmkv (persisted state)
- @tanstack/react-query v5
- axios
- i18next + react-i18next + react-native-localize (8 languages: en/ar/de/es/hi/ja/ru/zh, RTL Arabic)
- react-native-reanimated, react-native-gesture-handler
- react-native-fast-image
- react-native-image-picker
- react-native-geolocation-service + react-native-permissions
- react-native-device-info
- react-native-webview
- react-native-linear-gradient
- react-native-vector-icons (MaterialIcons + Ionicons)
- react-native-svg
- @shopify/flash-list
- @react-native-firebase/app + auth + messaging
- react-hook-form + zod
- react-native-maps

Brand:
- App name: SwiftMart
- Primary: #6133BD  |  Secondary: #2C91FE
- Dark Primary: #7E57C2  |  Dark Secondary: #4AB0FF
- Bundle ID: com.swiftmart.customer

API (all POST application/x-www-form-urlencoded):
- Base URL: set in src/lib/config.ts as AppConfig.baseUrl
- Path: /app/v1/api/
- Auth: Authorization: Bearer <jwt>
- Response envelope: { error: boolean, message: string, data: T }
- Pagination: { error, message, total, offset, limit, data: T[] }

=== FILES TO CREATE ===

CONFIG FILES (project root):
- package.json (add all packages above)
- tsconfig.json (strict, paths: @/* → src/*, @assets/* → assets/*)
- babel.config.js (@react-native/babel-preset + module-resolver + reanimated)
- metro.config.js
- react-native.config.js

SRC STRUCTURE:

src/lib/
- config.ts       → AppConfig.baseUrl, apiUrl(), AppConfig.shareWebUrl
- api.ts          → axios instance, post<T>(), postMultipart(), ApiException
- endpoints.ts    → all 80+ API method name constants (Api.login, Api.placeOrder…)
- format.ts       → formatPrice(), toNum(), variantPrice(), discountPercent(), dateLabel()
- i18n.ts         → i18next setup with react-native-localize, RTL for Arabic
- storage.ts      → MMKV instance, StorageKeys, jsonGet/jsonSet helpers
- queryClient.ts  → staleTime 60s, gcTime 5min, retry 1

src/types/models.ts → interfaces:
User, Product, ProductVariant, Category, Brand, Section, Slider,
FlashSale, CartItem, CartResponse, Address, Order, OrderItem,
PromoCode, Transaction, Withdrawal, NotificationItem, Rating,
Ticket, TicketMessage, AppSettings, SystemSettings, PaymentSettings, DeliveryTimeSlot

src/stores/
- authStore.ts    → user, token (MMKV), isLoggedIn(), setSession(), logout()
- cartStore.ts    → guestLines (MMKV), addLine/updateQty/removeLine/clear, cartCount
- themeStore.ts   → mode light/dark/system (MMKV), useColors() hook
src/theme/colors.ts → palette, lightColors, darkColors, ThemeColors interface

src/features/ (TanStack Query hooks + API calls):
- settings/api.ts → fetchSettings(), useSettings(), useSystemSettings(), usePaymentSettings(), useCurrency()
- auth/api.ts     → verifyUser(), login(), registerUser(), resetPassword(), socialSignUp(), validateReferCode(), updateProfile(), deleteAccount()
- home/api.ts     → useSliders(), useOfferImages(), usePopupOffer(), useSections()(infinite), useFlashSales()
- catalog/api.ts  → useCategories(parentId?), useProducts(filters)(infinite), useProductDetail(id), useBrands(), useProductFaqs(id)
- wishlist/api.ts → useFavorites(), useToggleFavorite()
- cart/api.ts     → useServerCart(), useManageCart(), useRemoveFromCart(), mergeGuestCart()
- address/api.ts  → useAddresses(), useSaveAddress(), useUpdateAddress(), useDeleteAddress(), checkCartDeliverable(), getCities(), getAreas()
- checkout/api.ts → validatePromo(), placeOrder(), addTransaction(), deleteOrder()
- payments/handlers.ts → PaymentHandler interface, registry: COD/wallet/bankTransfer/webViewGateway/nativeStubs (Razorpay/Stripe/Paystack/Paytm/PhonePe), enabledHandlers(), finalizeGatewayPayment(), resolvePendingPayment()
- orders/api.ts   → useOrders(filter?)(infinite), useOrderDetail(), useUpdateOrderStatus(), useUpdateOrderItemStatus(), sendBankTransferProof(), useTransactions(), useWithdrawals(), sendWithdrawalRequest()
- support/api.ts  → useTicketTypes(), useTickets(), useCreateTicket(), useTicketMessages()(10s poll), useSendTicketMessage()
- reviews/api.ts  → useProductRatings(id), addProductRating(multipart)

src/navigation/
- types.ts          → RootStackParamList, AuthStackParamList, TabParamList
- hooks.ts          → useNavigation() typed, useNavigateToTabs(), useNavigateToAuth()
- RootNavigator.tsx → NavigationContainer > NativeStack (all 30 screens registered)
- AuthNavigator.tsx → Login, SignUp, SendOtp, VerifyOtp, SetPassword
- MainTabNavigator.tsx → Home🏠 / Categories🗂️ / Cart🛒 (with badge) / Wishlist❤️ / Profile👤

src/components/
- AppButton.tsx      → primary/outline, loading spinner, disabled
- AppTextInput.tsx   → label, error msg, left/right icon slots
- ProductCard.tsx    → image, discount badge, price, rating stars, navigate ProductDetail
- ProductListScreen.tsx → FlashList 2-col grid, infinite scroll, sort bottom sheet

src/features/home/components/
- SliderCarousel.tsx   → auto-play 4s, pagination dots, type-based navigation
- FlashSaleStrip.tsx   → live HH:MM:SS countdown per sale, navigate FlashSale screen
- FeaturedSection.tsx  → style_1 (banner+rail) / style_2 (grid) / style_3 (hero+grid) / default (rail)

=== ALL SCREENS (complete UI + API + loading + error states) ===

Auth screens (src/screens/auth/):
- SplashScreen.tsx      → fetchSettings, maintenance gate, version check (DeviceInfo.getVersion), navigate Tabs/Auth
- LoginScreen.tsx       → phone or email toggle, JWT to MMKV, mergeGuestCart after login, CommonActions.reset to Tabs
- SignUpScreen.tsx       → username/email/mobile/password, referral code, login after register
- SendOtpScreen.tsx     → request OTP via Firebase phone auth
- VerifyOtpScreen.tsx   → 6-digit OTP input, confirm with Firebase auth, navigate SetPassword
- SetPasswordScreen.tsx → set new password after OTP verify

Main tabs (src/screens/):
- home/HomeScreen.tsx       → sticky search bar, SliderCarousel, categories strip (scroll), FlashSaleStrip, offer images row, FeaturedSections paginated, popup offer modal (24h guard in MMKV)
- categories/CategoriesScreen.tsx → 3-col grid, tap → SubCategories (has children) or CategoryProducts
- cart/CartScreen.tsx           → if logged in: ServerCart with QtyStepper; else: GuestCart; Proceed button
- wishlist/WishlistScreen.tsx   → grid of favorited products, toggle remove
- profile/ProfileScreen.tsx     → user info, wallet balance, menu list (requiresAuth gating), edit profile, logout

Product screens (src/screens/product/):
- ProductDetailScreen.tsx → image gallery swiper, variant chips, price chain display, add-to-cart (server or guest), share button, favorite toggle, similar products rail, sticky bottom bar (price + buy button), useLayoutEffect for title

Category screens (src/screens/category/):
- CategoryProductsScreen.tsx → ProductListScreen with category filter
- SubCategoriesScreen.tsx    → grid of child categories

Search + Listing:
- SearchScreen.tsx    → debounced 400ms, ProductListScreen
- SectionScreen.tsx   → section products
- FlashSaleScreen.tsx → flash sale products with countdown

Checkout flow:
- checkout/CheckoutScreen.tsx → delivery type (door/pickup), address picker (+ add new), time slots, promo code apply, wallet toggle, payment method radio list (from enabledHandlers()), order note, totals breakdown, placeOrder() with deliverability pre-check + gateway payment
- checkout/OrderSuccessScreen.tsx → celebration, order ID, view order / continue shopping
- PaymentWebViewScreen.tsx → WebView for PayPal/Flutterwave/Instamojo/Midtrans/MyFatoorah, detect success/failure URL

Address screens (src/screens/address/):
- AddressListScreen.tsx → list with type badge, default badge, edit/delete
- AddAddressScreen.tsx  → type chips (Home/Office/Others), GPS locate button (react-native-geolocation-service + react-native-permissions), all address fields, is_default switch, edit mode from route.params.edit

Order screens (src/screens/orders/):
- OrdersScreen.tsx   → filter chips (all/received/processed/shipped/delivered/cancelled/returned), StatusBadge component
- OrderDetailScreen.tsx → items with cancelable_till gate, bank proof upload (react-native-image-picker), OTP display, order totals, status timeline

User screens:
- WalletScreen.tsx        → balance card, wallet transaction list, withdrawal form toggle (amount + payAddress)
- TransactionsScreen.tsx  → credit/debit list (credit=green +, debit=red -)
- ReferEarnScreen.tsx     → referral code in dashed box, bonus info from systemSettings, Share.share()
- PromoCodesScreen.tsx    → promo code cards with discount type, min order, end date, cashback badge
- NotificationsScreen.tsx → notification list with image support, date
- FaqsScreen.tsx          → accordion (tap question → expand/collapse answer)
- PolicyScreen.tsx        → route.params.type → privacy_policy/terms_conditions/about_us, strip HTML
- ProfileEditScreen.tsx   → username, email, old+new password (optional), multipart update_user

Support screens (src/screens/support/):
- SupportScreen.tsx      → ticket list with STATUS_LABEL map, create ticket modal (type + subject + description)
- TicketThreadScreen.tsx → inverted FlatList chat, mine=right/primary, admin=left/card, composer row

Reviews:
- ReviewsScreen.tsx → rating stars, comment, images, user name, date

=== KEY BUSINESS LOGIC ===

Guest cart: MMKV → merge on login via mergeGuestCart()
cancelable_till: STAGE_ORDER=['received','processed','shipped','delivered']; canCancelItem checks index
Flash price: flash_sale_price > special_price > price (toNum() > 0 check)
Settings bootstrap: SplashScreen fetches settings → maintenance → version check → navigate
Payment registry: enabledHandlers(paymentSettings, codAllowed) → handler.pay() → finalizeGatewayPayment()
```

---

### Step 4 — Say `continue` until done

Claude will write all files in order. Every time it stops:
```
continue
```

### Step 5 — Install packages

```bash
npm install
cd ios && pod install && cd ..
```

### Step 6 — Native permissions setup

**Android** — add to `android/app/src/main/AndroidManifest.xml`:
```xml
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.INTERNET" />
```

**iOS** — add to `ios/SwiftMart/Info.plist`:
```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>Used to auto-fill your delivery address</string>
<key>NSPhotoLibraryUsageDescription</key>
<string>Used to upload product photos and bank transfer proof</string>
<key>NSCameraUsageDescription</key>
<string>Used to take photos for reviews</string>
```

### Step 7 — Firebase setup (for OTP + Push)

1. Go to [console.firebase.google.com](https://console.firebase.google.com)
2. Create project → enable **Phone Auth** + **Cloud Messaging**
3. Android: download `google-services.json` → place in `android/app/`
4. iOS: download `GoogleService-Info.plist` → add to Xcode project
5. Follow `@react-native-firebase` setup docs for Android `build.gradle` changes

### Step 8 — Point app to your backend

In `src/lib/config.ts` set your backend URL:
```ts
export const AppConfig = {
  baseUrl: 'http://YOUR_LOCAL_IP:3000/',   // e.g. http://192.168.1.10:3000/
  // For production: 'https://api.yourdomain.com/'
};
```

> Use your **LAN IP** (not localhost) when testing on a physical device.

### Step 9 — Run

```bash
# Android
npm run android

# iOS
npm run ios
```

---

## Part 2 — Backend + Admin Panel (SwiftMartBackend)

### Step 1 — Create the folder

```bash
mkdir SwiftMartBackend
cd SwiftMartBackend
```

### Step 2 — Set up MySQL

```sql
CREATE DATABASE swiftmart_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### Step 3 — Open Claude Code

```bash
claude
```

### Step 4 — Paste this prompt

---

```
Build a production-ready e-commerce backend + admin panel called SwiftMartBackend.

Two sub-projects:
- server/   → Node.js + Express REST API (pure API, no UI)
- admin/    → Next.js 15 admin panel that ALSO embeds the Express API on the same port

=== SERVER (server/) — Pure API ===

Tech:
- Node.js 20 + Express 4 + TypeScript 5 strict
- Prisma 6 (MySQL provider) — run prisma db push to create tables
- jsonwebtoken — JWT (RS256 or HS256)
- bcryptjs — password hashing
- multer — disk storage to uploads/ folder
- sharp — image resize on upload
- dotenv — .env config
- zod — request body validation
- helmet + cors + morgan
- firebase-admin — FCM push notifications

Two JWT secrets:
- JWT_SECRET → mobile user tokens
- ADMIN_JWT_SECRET → admin panel tokens

Response helpers (attach to res in middleware):
- res.ok(data, message?)    → { error: false, message, data }
- res.fail(message, code?)  → { error: true, message, data: [] }
- res.paginate(data, total, offset, limit)

Middleware:
- verifyToken     → parse Bearer token, attach req.user (from users table)
- optionalToken   → attach req.user if token present, continue if not
- verifyAdmin     → parse Bearer token with ADMIN_JWT_SECRET, attach req.admin
- upload          → multer disk storage, field name 'image', also 'attachments'

Prisma schema (all tables in MySQL):
users, users_groups, addresses, cities, areas, zipcodes,
categories (with parent_id self-relation),
products, product_variants, product_faqs,
brands, attributes, attribute_sets, attribute_values,
flash_sales, flash_sale_products,
sections, section_products,
sliders, offer_images, popup_offers,
cart_items, wishlist,
orders, order_items, order_status_history,
transactions, withdrawals,
promo_codes,
reviews, review_images,
notifications, fcm_tokens,
tickets, ticket_messages, ticket_types,
faqs,
system_settings, payment_settings, delivery_time_slots,
taxes, return_reasons, return_requests,
delivery_boys, pickup_locations,
client_api_keys, media_library

=== MOBILE API ROUTES (POST /app/v1/api/:action) ===

All form-urlencoded. Response: { error: boolean, message: string, data: T }

Auth:
- /login                    → email or mobile + password, fcm_id, return JWT + user
- /sign_up                  → username, email, mobile, password, referral_code
- /verify_user              → check if mobile/email exists
- /verify_otp               → Firebase token verify (or OTP table fallback)
- /reset_password           → mobile + new password
- /get_login_identity       → return user info by token
- /validate_refer_code      → check referral code valid
- /update_user              → multipart, update profile fields + image
- /delete_user              → soft delete (is_active=0)
- /social_sign_up           → firebase_uid, login_type (google/apple)

Catalog:
- /get_settings             → system_settings + payment_settings + time_slots merged
- /get_categories           → parent_id? filter, nested children count
- /get_products             → category_id?, brand_id?, search?, sort, order, limit, offset
- /get_product_details      → product + variants + flash price + avg_rating
- /get_brands_data          → active brands list
- /get_slider_images        → active sliders ordered
- /get_offer_images         → offer image banners
- /get_popup_offer_images   → popup modal offer
- /get_sections             → paginated sections with products
- /get_flash_sale           → active flash sales with products + flash prices
- /get_product_rating       → reviews with pagination + user can_edit flag
- /get_product_review_images → all images for a product's reviews
- /get_product_faqs         → Q&A for a product
- /add_product_faqs         → user submits a product question
- /get_faqs                 → general FAQs list

Cart & Wishlist:
- /get_user_cart            → cart items with variant + product data + delivery_charge
- /manage_cart              → add/update qty (upsert by product_variant_id)
- /remove_from_cart         → delete by cart_id
- /get_favorites            → user wishlist
- /add_to_favorites         → toggle add
- /remove_from_favorites    → remove by product_id

Address & Delivery:
- /get_address              → user addresses list
- /add_address              → create new address
- /update_address           → edit existing
- /delete_address           → remove address
- /get_cities               → cities list
- /get_areas_by_city_id     → areas for a city
- /get_zipcodes             → zipcodes
- /is_product_delivarable   → check single product deliverable to pincode
- /check_cart_products_delivarable → check all cart items deliverable to address_id

Orders:
- /place_order              → full order placement:
                              - check stock for each variant
                              - decrement stock
                              - deduct wallet if is_wallet_used=1
                              - create order + order_items
                              - apply promo cashback if applicable
                              - credit referral bonus to referrer on first order
                              - send FCM to user
- /add_transaction          → record payment transaction (txn_id, method, status)
- /get_orders               → paginated, active_status filter
- /get_order_details        → order + items + address + status history + otp
- /update_order_status      → user cancel: whole order (if status=received + all items cancellable)
- /update_order_item_status → user cancel item: check cancelable_till gate
- /delete_order             → delete failed/draft order
- /send_bank_transfer_proof → multipart upload proof image for order

Promos & Wallet:
- /validate_promo_code      → check code, dates, min_order, return discount amount
- /get_promo_codes          → active promo codes list
- /transactions             → type=wallet or transaction, paginated
- /get_withdrawal_request   → user withdrawal history
- /send_withdrawal_request  → request wallet withdrawal (pay_address + amount)

Support:
- /get_ticket_types         → ticket category list
- /get_tickets              → user tickets paginated
- /add_ticket               → create ticket (type_id, subject, description)
- /edit_ticket              → update ticket status
- /get_messages             → ticket message thread paginated
- /send_message             → send message (text + optional attachments[])

Notifications:
- /get_notifications        → user notifications paginated
- /mark_notification_read   → mark single or all as read

Payments (gateway WebViews — GET routes):
- GET /get_paypal_link      → return PayPal redirect URL
- GET /flutterwave_webview  → Flutterwave page
- GET /instamojo_webview    → Instamojo page
- GET /midtrans_webview     → Midtrans page
- GET /myfatoorah_webview   → MyFatoorah page
- POST /generate_paytm_txn_token  → Paytm token
- POST /create_midtrans_transaction → Midtrans token
- GET /payment-success      → WebView success landing
- GET /payment-cancel       → WebView cancel landing

Ratings:
- /set_product_rating       → multipart: rating, comment, images[] (up to 6), update product avg
- /delete_product_rating    → user delete own review

=== ADMIN API ROUTES (/admin/v1/api/*) ===

Auth:
- POST /login               → email + password, check users_groups group_id=1, return admin JWT

Dashboard:
- GET /dashboard            → { total_orders, total_revenue, total_users, total_products, today_orders, recent_orders[], order_status_breakdown[] }
- GET /reports/sales?from&to   → { total_orders, total_revenue, avg_order_value, series: [{date, orders, revenue}] }
- GET /reports/products?from&to → [{ name, qty, revenue }] top products

Products CRUD:
- GET /products?search&category_id&page&limit
- POST /products            → multipart (image + other_images JSON)
- PUT /products/:id         → multipart
- DELETE /products/:id
- GET /products/:id/variants
- POST /products/:id/variants
- PUT /products/variants/:id
- DELETE /products/variants/:id

Categories:
- GET/POST/PUT/DELETE /categories

Brands, Attributes, Attribute Sets, Attribute Values:
- Full CRUD for each

Sliders, Offer Images, Popup Offers:
- GET/POST/PUT/DELETE each (image upload)

Sections + Section Products:
- CRUD sections, add/remove/reorder products

Flash Sales:
- GET/POST/PUT/DELETE flash-sales
- POST/DELETE /flash-sales/:id/products (add variant with flash price)

Orders:
- GET /orders?status&page&limit
- GET /orders/:id           → full detail (items, address, history, proof image)
- PUT /orders/:id/status    → { status }
- PUT /orders/:id/items/:itemId/status
- PUT /orders/:id/bank-proof-status → approve/reject bank transfer

Users:
- GET /users?search&page&limit
- GET /users/:id
- PUT /users/:id/status     → activate/deactivate
- PUT /users/:id/balance    → credit or debit wallet manually

Reviews:
- GET /reviews?product_id
- DELETE /reviews/:id

Promo Codes:
- GET/POST/PUT/DELETE /promo-codes

FAQs:
- GET/POST/PUT/DELETE /faqs

Ticket Types:
- GET/POST/PUT/DELETE /ticket-types

Support Tickets (admin side):
- GET /tickets?status
- GET /tickets/:id          → with message thread
- POST /tickets/:id/reply   → admin sends message
- PUT /tickets/:id/status

Notifications:
- GET /notifications
- POST /notifications/send  → { title, message, image?, user_id? (null=broadcast) } → FCM

Withdrawals:
- GET /withdrawals?status
- PUT /withdrawals/:id      → { status: approved|rejected } → on approved: credit wallet + create transaction

Settings:
- GET/PUT /settings/system  → multipart (logo images, currency, maintenance, versions, min_cart)
- GET/PUT /settings/payment → all gateway toggles + credentials
- GET/PUT /settings/time-slots

Delivery Boys:
- GET/POST/PUT/DELETE /delivery-boys

Pickup Locations:
- GET/POST/PUT/DELETE /pickup-locations

Taxes:
- GET/POST/PUT/DELETE /taxes

Return Reasons + Return Requests:
- GET/POST/PUT/DELETE /return-reasons
- GET /return-requests, PUT /return-requests/:id/status

Media Library:
- GET /media?page&limit
- DELETE /media/:id

=== ADMIN (admin/) — Next.js 15 on same port as API ===

Tech:
- Next.js 15 App Router + TypeScript 5
- server.ts → combines Express API (mountApi) + Next.js handle → single process on PORT=3000
- Admin auth: JWT in localStorage (no next-auth — simpler)
- Custom CSS (globals.css) — no Tailwind needed
- All charts: inline SVG (no external chart lib)
- All components: pure React + inline styles

App Router structure:
app/
├── layout.tsx                       → root layout (globals.css)
├── page.tsx                         → redirect to /dashboard or /login
├── login/page.tsx                   → email + password form, POST /admin/v1/api/login, store JWT
└── (dashboard)/
    ├── layout.tsx                   → sidebar + main area + auth guard
    ├── dashboard/page.tsx           → KPI cards, revenue area chart (SVG), order status bars, recent orders table
    ├── reports/page.tsx             → date range presets + custom, trend chart toggle (revenue/orders), top products ranked bars
    ├── products/
    │   ├── page.tsx                 → searchable table, image thumbs
    │   └── [id]/page.tsx            → product form + variant table CRUD
    ├── categories/page.tsx          → tree view, parent selector, image upload
    ├── brands/page.tsx
    ├── attributes/page.tsx
    ├── attribute-sets/page.tsx
    ├── attribute-values/page.tsx
    ├── orders/
    │   ├── page.tsx                 → status filter tabs, table
    │   └── [id]/page.tsx            → order detail: items, address, proof image, status update, history timeline
    ├── users/
    │   ├── page.tsx
    │   └── [id]/page.tsx            → user detail: orders, wallet, activate/deactivate, manual balance
    ├── sliders/page.tsx
    ├── offer-sliders/page.tsx
    ├── popup-offers/page.tsx
    ├── sections/page.tsx
    ├── flash-sales/page.tsx
    ├── promo-codes/page.tsx
    ├── reviews/page.tsx
    ├── notifications/page.tsx       → send broadcast or targeted FCM
    ├── support/
    │   ├── page.tsx
    │   └── [id]/page.tsx            → chat thread + admin reply
    ├── faqs/page.tsx
    ├── withdrawals/page.tsx         → approve→wallet credit, reject
    ├── delivery-boys/page.tsx
    ├── pickup-locations/page.tsx
    ├── time-slots/page.tsx
    ├── taxes/page.tsx
    ├── return-reasons/page.tsx
    ├── return-requests/page.tsx
    ├── media/page.tsx
    └── settings/page.tsx            → tabs: General / Payment / Policies

Components:
components/
├── Sidebar.tsx  → grouped nav links + section labels + logout + active state
├── ui.tsx       → ToastProvider, useToast(), TopBar, Modal
└── CrudResource.tsx → generic CRUD list+form pattern

lib/
├── api.ts       → adminGet/adminPost/adminPut/adminDelete/adminUpload, getToken/setToken/clearToken
└── auth.ts      → JWT decode helper

Admin design system (globals.css):
- Sidebar: dark bg #0f172a, text #e2e8f0, accent #6366f1
- Content: bg #f1f5f9, cards white
- Status colors: received=#0369a1(blue), processed=#92400e(amber), shipped=#6d28d9(purple), delivered=#15803d(green), cancelled=#b91c1c(red)
- SVG charts: smooth bezier curves, gradient area fill, grid lines, hover tooltip dark card
- Skeleton loading: @keyframes pulse opacity
- Toast: fixed bottom-right, dark bg, red for errors

Build in order:
1. server/package.json + tsconfig.json
2. server/prisma/schema.prisma
3. server/src/config/ (env.ts, prisma.ts, firebase.ts)
4. server/src/middleware/ (response.ts, auth.ts, upload.ts)
5. server/src/utils/ (jwt.ts, params.ts, media.ts, fcm.ts)
6. server/src/services/ (all services: auth, catalog, cart, order, misc, location, review, support, wallet, settings, admin)
7. server/src/routes/ (index.ts all mobile routes, admin.routes.ts all admin routes)
8. server/src/app.ts + server.ts
9. admin/package.json + tsconfig.json + next.config.ts
10. admin/prisma/ (copy schema, generate client)
11. admin/app/ (all pages in order above)
12. admin/components/ (Sidebar, ui, CrudResource)
13. admin/lib/ (api.ts, auth.ts)
14. admin/server.ts (unified entry)
15. server/.env.example + admin/.env.example
```

---

### Step 5 — Set up environment variables

Create `server/.env` and `admin/.env` (both need the same values):

```env
# Database
DATABASE_URL="mysql://root:YOUR_PASSWORD@127.0.0.1:3306/swiftmart_db"

# JWT secrets — use long random strings, different for each
JWT_SECRET=paste_a_long_random_string_here_for_users
ADMIN_JWT_SECRET=paste_a_different_long_random_string_here_for_admin

# Server
PORT=3000
NODE_ENV=development

# Your server's public URL (used to build image URLs)
BASE_URL=http://localhost:3000/

# CORS (admin URL — same origin when unified)
ADMIN_URL=http://localhost:3000
```

> **Security:** Never commit `.env` files. Add `.env` to `.gitignore` immediately.

### Step 6 — Install and run

```bash
# Server
cd server
npm install
npx prisma db push        # creates all tables from schema
npx prisma generate       # generates Prisma client
npm run dev               # API live on :3000

# Admin (different terminal)
cd ../admin
npm install
npx prisma generate
npm run dev               # Next.js + API unified on :3000
```

### Step 7 — Create first admin account

Generate a password hash:
```bash
node -e "require('bcryptjs').hash('Admin@123', 12, (e,h)=>console.log(h))"
```

Insert admin user into DB:
```sql
-- First insert the user
INSERT INTO users (username, email, password, mobile, is_active, created_at)
VALUES ('Administrator', 'admin@swiftmart.com', 'PASTE_BCRYPT_HASH_HERE', '9999999999', 1, NOW());

-- Then assign to admin group (group_id = 1 = admin)
INSERT INTO users_groups (user_id, group_id)
VALUES (LAST_INSERT_ID(), 1);
```

### Step 8 — Open admin panel

```
http://localhost:3000/login
Email:    admin@swiftmart.com
Password: Admin@123
```

---

## Full Feature List

### Mobile App (SwiftMart)
- [ ] Phone/email login + Firebase OTP password reset
- [ ] Google + Apple social login (via Firebase)
- [ ] Guest cart in MMKV → merges to server on login
- [ ] Product search with 400ms debounce
- [ ] Flash sales with live HH:MM:SS countdown per sale
- [ ] 4 homepage section styles (banner+rail, grid, hero+grid, rail)
- [ ] Popup offer modal with 24h cooldown guard
- [ ] Variant selector (size/color/weight/etc.)
- [ ] Area-based delivery availability check per product
- [ ] Address management with GPS auto-fill
- [ ] 10+ payment methods (COD, Wallet, Bank Transfer, PayPal, Razorpay, Stripe, Flutterwave, Instamojo, Midtrans, MyFatoorah, Paystack, Paytm, PhonePe)
- [ ] Order tracking with full status timeline
- [ ] Order item cancellation with stage gate (cancelable_till)
- [ ] Bank transfer proof image upload
- [ ] Wallet with credit/debit history
- [ ] Withdrawal requests from wallet
- [ ] Promo codes (flat, percentage, cashback to wallet)
- [ ] Support ticket chat with admin (10s polling)
- [ ] Product reviews with multiple photos
- [ ] Product Q&A (add questions, see answers)
- [ ] FCM push notifications (order updates, broadcasts)
- [ ] Referral system (code share, bonus on referral's first order)
- [ ] 8 languages: English, Arabic (RTL), German, Spanish, Hindi, Japanese, Russian, Chinese
- [ ] Dark mode + system theme sync
- [ ] Return requests for delivered orders

### Backend API (SwiftMartBackend/server)
- [ ] 65+ mobile REST endpoints
- [ ] 117+ admin REST endpoints
- [ ] JWT auth (separate user + admin secrets)
- [ ] Image upload + resize via Multer + Sharp
- [ ] Firebase Admin FCM push (single user + broadcast)
- [ ] Inventory check + decrement on order
- [ ] Wallet transaction engine (credit, debit, referral bonus)
- [ ] Payment gateway WebView redirect URLs
- [ ] Paytm + Midtrans native token generation
- [ ] Promo code engine (flat, %, cashback, date validation)
- [ ] Delivery area availability check
- [ ] Product rating aggregation (auto-recalculate avg after review)
- [ ] Referral bonus on referrer's first order delivery
- [ ] Soft deletes (is_active = 0)
- [ ] File served from /uploads/ as static

### Admin Panel (SwiftMartBackend/admin)
- [ ] Dashboard: KPI cards + SVG revenue chart + order status bars + recent orders
- [ ] Sales reports with date presets + custom range + chart toggle (revenue/orders)
- [ ] Top products ranked bar chart
- [ ] Products CRUD with multiple variants
- [ ] Categories CRUD (parent/child nested)
- [ ] Brands, Attributes, Attribute Sets, Attribute Values CRUD
- [ ] Orders management: filter by status, full order detail, update status
- [ ] Order bank transfer proof review + approve/reject
- [ ] Users management: wallet credit/debit, activate/deactivate
- [ ] Flash sales scheduler (date range, per-variant prices)
- [ ] Sliders, Offer Banners, Popup Offers CRUD
- [ ] Sections + product assignment + ordering
- [ ] Promo codes CRUD (flat/% toggle, cashback, date range)
- [ ] Support ticket system: full chat thread, admin reply, status update
- [ ] Push notification broadcaster (all users or target user)
- [ ] Withdrawal approvals → automatic wallet credit on approve
- [ ] Settings: General (logo, currency, maintenance mode, min app version) / Payment (gateway toggles + credentials) / Policies (privacy/terms/about)
- [ ] Delivery boys management
- [ ] Pickup locations CRUD
- [ ] Time slots CRUD
- [ ] Taxes CRUD
- [ ] Return reasons + return request management
- [ ] Media library
- [ ] Animated skeleton loaders + toast notifications

---

## Tips for Best Results with Claude

### Be specific — not vague
```
❌ "make a login screen"
✅ "Build LoginScreen.tsx: phone/email toggle, react-hook-form + zod validation,
   POST to /login, store JWT in MMKV via setSession(), mergeGuestCart() on success,
   CommonActions.reset to Tabs"
```

### When Claude stops mid-file
```
continue
```

### When you get an error
Paste the full error. Claude will fix it:
```
Getting this error: [paste full error text]
Fix it.
```

### Build order matters — always do this sequence
```
1. package.json + tsconfig + babel config
2. Types (models.ts)
3. Lib layer (config, api, endpoints, format, storage)
4. Stores (auth, cart, theme)
5. Feature hooks (API calls)
6. Navigation types + navigators
7. Components
8. Screens (Splash → Auth → Tabs → Stack screens)
```

### Keep context alive
One long session is better than many short ones. Claude remembers what it built earlier in the same session.

---

## Common Mistakes to Avoid

| ❌ Wrong | ✅ Right |
|---|---|
| Use Expo | React Native CLI — no Expo |
| React Router in mobile app | React Navigation 7 |
| `router.push('/path')` (Expo Router) | `navigation.navigate('ScreenName', params)` |
| `useLocalSearchParams()` | `useRoute<RouteProp<...>>().params` |
| AsyncStorage for auth token | react-native-mmkv (10x faster, encrypted) |
| `useNavigation()` without generic | `useNavigation<RootNav>()` from `@/navigation/hooks` |
| Axios in Next.js Server Components | Native `fetch()` (no axios on server side) |
| One JWT secret for everyone | Separate `JWT_SECRET` (users) + `ADMIN_JWT_SECRET` (admin) |
| Hand-write Prisma schema from scratch | Run `prisma db pull` from existing DB |
| `npm run dev` server + admin same port | Admin/unified server handles both — one process |
| Commit `.env` to git | Add `.env` to `.gitignore` immediately |
| `localhost` in mobile app base URL | Use LAN IP (`192.168.x.x`) for physical device |

---

## Deployment

### Generate secure secrets
```bash
node -e "console.log(require('crypto').randomBytes(64).toString('hex'))"
# Run twice — one for JWT_SECRET, one for ADMIN_JWT_SECRET
```

### Backend on Hostinger VPS / any Node server

```bash
# On server
git clone your-repo
cd SwiftMartBackend/admin   # or server if separate
npm install
npx prisma generate
npm run build
pm2 start server.ts --name swiftmart --interpreter tsx
pm2 save
pm2 startup
```

Nginx config:
```nginx
server {
  listen 80;
  server_name api.yourdomain.com;

  location / {
    proxy_pass http://localhost:3000;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    client_max_body_size 20M;
  }

  location /uploads/ {
    alias /var/www/swiftmart/uploads/;
    expires 30d;
  }
}
```

### Mobile App → Stores

```bash
# Android → APK / AAB for Play Store
cd android
./gradlew bundleRelease    # .aab file for Play Store
./gradlew assembleRelease  # .apk for direct install

# iOS → App Store
# Open ios/SwiftMart.xcworkspace in Xcode
# Product → Archive → Distribute App
```

---

## Resources

| Resource | Link |
|---|---|
| Claude Code | claude.ai/code |
| Fable 5 model ID | `claude-fable-5` |
| React Native docs | reactnative.dev |
| React Navigation | reactnavigation.org |
| Prisma docs | prisma.io/docs |
| Next.js docs | nextjs.org/docs |
| Firebase Console | console.firebase.google.com |
| TanStack Query | tanstack.com/query |

---

*Built with Claude Fable 5 — one prompt, production-ready code.*
