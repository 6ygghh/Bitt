# Telegram XLSX Bot V2

User menu:
Account
Pending Accounts | Sell Account
Withdraw | Support
Price List

Deposit button নেই। Buttons-এ emoji নেই।

শুধু ADMIN_ID-এর account-এ Main Menu-তে Admin Panel button দেখা যাবে।

Account button: User ID + Balance + Today's Income + Total Income দেখায়।

Sell Account: Work নির্বাচন -> XLSX পাঠানো -> প্রথম non-empty row header হিসেবে বাদ -> account count -> price অনুযায়ী estimated profit -> user confirmation -> admin receives original file with username, ID, work, count, price, file name and time.

Admin Pending Files: সব pending file-এর username, ID, work, account count, value, file name, time এবং approve/reject command দেখায়।

Withdrawal Management:
ADD|bKash|100|5000
UPDATE|ID|bKash|200|10000
OFF|ID
ON|ID

User Withdraw: category -> amount -> wallet. Amount আগে balance থেকে reserve হবে. Reject হলে ফেরত যাবে.

Support Management: শুধু username লিখুন, যেমন support_username. User Support button-এ সেটি দেখাবে.

Railway Variables:
BOT_TOKEN = BotFather token
ADMIN_ID = numeric Telegram user ID

Production-এ Railway persistent volume ব্যবহার করুন bot.db এবং uploads-এর জন্য।


## New User Management

Admin Panel -> User Management চাপলে:
- All User List
- Block User
- Blocked User
- Add Balance
- Remove Balance

Block User-এ User ID দিলে user blocked হবে এবং bot ব্যবহার করতে পারবে না। Admin account নিজে block হবে না।


## Force Join

Admin Panel -> Force Join:
- Add Force Join Channel
- Force Join Channel List
- Remove Force Join Channel

Add format:
CHANNEL_ID|CHANNEL_NAME|INVITE_LINK

Example:
-1001234567890|My Channel|https://t.me/mychannel

Important:
1. Bot-কে প্রতিটি required channel-এর Admin করতে হবে।
2. Channel ID সঠিক হতে হবে।
3. User bot-এর feature ব্যবহার করার আগে required channel-গুলোতে joined কিনা check হবে।
4. User না joined হলে Join button এবং Check Joined button দেখাবে।
5. Admin Force Join check থেকে bypass করবে।


## Set Welcome Message

Admin Panel -> Set Welcome Message চাপুন। এরপর আপনার Welcome message পাঠান।
মেসেজটি database-এ save হবে এবং User `/start` করলে (এবং Force Join থাকলে সব required channel join করার পর) সেই Welcome message পাঠানো হবে।

পরবর্তীতে আবার Set Welcome Message চাপলে আগের message replace হয়ে যাবে।


## V6 UI Update

User main menu:
- Sell Account
- Pending Accounts
- Withdraw
- Price List

The old Account and Support buttons are removed from the main user menu.

### Premium button-based admin setup
Admin no longer needs to type pipe-delimited commands for:
- Force Join channel setup
- Work / Price setup

Use the buttons and follow the step-by-step prompts instead.


## V8 User Panel Fix

The User Panel buttons are now handled independently and the Admin account can also test them.
Main User Menu:
- Account
- Pending Account | Sell Account
- Withdrawal | Price List
- Support

The previous issue where the generic Admin handler swallowed user-menu button presses has been fixed.

Admin Panel now uses a compact premium 2-column button layout.

For testing the User Menu from the admin account, use `/userpanel`.


## V9 Category Selection Fix

Sell Account and Withdrawal category selections now have dedicated high-priority handlers.
- Work/Price category button selection is normalized for dash/spacing differences.
- Withdrawal category selection is normalized for dash/spacing differences.
- Cancel always returns to the User Menu.
- After Sell Account category selection, the keyboard is removed and the bot waits for XLSX.
- After Withdrawal category selection, the bot asks for the amount.


## V10 Support Message

When a user presses Support, the bot shows the exact Bengali customer-service message and an inline `📞 Contact Support` button.

The button uses the support link/username already configured in Admin Panel -> Support Management.
Accepted formats:
- `@username`
- `t.me/username`
- `https://t.me/username`


## V12 Sell Account Fix

Fixed the case where selecting a Sell Account Work/Price button produced no response, especially when testing the User Panel from the Admin account. The selected category is now handled before the generic Admin state handler, then the bot explicitly asks for the XLSX file. Withdrawal category selection receives the same priority treatment.

## Premium Emoji UI v6
- Normal Unicode emoji that were mapped to custom emoji are replaced, not duplicated.
- Text labels remain intact (e.g. Your Accounts, Estimated Profit, User ID, Balance).
- Key labels are sent bold using Telegram message entities.
- Withdrawal/payment amount displays include the Bangla unit `টাকা`.
- Native Telegram reply-keyboard buttons do not support custom-emoji entities or background-color styling. The user menu therefore uses colored Unicode visual cues on buttons while message text uses real custom emojis.
