# Laravel Vue.js Real-time Chat Application

A real-time chat application built with **Laravel 5.5** and **Vue.js 2**, featuring real-time messaging capabilities using Pusher for instant message delivery.

## 📋 Table of Contents

- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Project Structure](#project-structure)
- [Usage](#usage)
- [Technologies](#technologies)
- [Development](#development)
- [API Endpoints](#api-endpoints)
- [Broadcasting](#broadcasting)
- [Contributing](#contributing)
- [License](#license)

## ✨ Features

- **Real-time Messaging**: Instant message delivery using Pusher broadcasting
- **User Authentication**: Secure login and registration system
- **Chat Interface**: Clean, responsive Vue.js-based chat UI
- **User Management**: Browse and chat with other users
- **Broadcasting Channels**: Channel-based message broadcasting
- **Responsive Design**: Bootstrap-powered responsive layout

## 📋 Prerequisites

Before you begin, ensure you have the following installed:

- **PHP** >= 7.0.0
- **Composer** (PHP package manager)
- **Node.js** and **npm** (for JavaScript dependencies)
- **Laravel 5.5**
- **Pusher Account** (for real-time messaging)

## 🚀 Installation

### 1. Clone the Repository

```bash
git clone <repository-url>
cd Laravel-vuejs-realtime-chat
```

### 2. Install PHP Dependencies

```bash
composer install
```

### 3. Install JavaScript Dependencies

```bash
npm install
```

### 4. Environment Setup

Copy the example environment file and generate an application key:

```bash
cp .env.example .env
php artisan key:generate
```

### 5. Configure Database

Edit your `.env` file with your database credentials:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel_chat
DB_USERNAME=root
DB_PASSWORD=
```

Then run migrations:

```bash
php artisan migrate
```

### 6. Configure Pusher (Broadcasting)

Update your `.env` file with Pusher credentials:

```env
PUSHER_APP_ID=your_app_id
PUSHER_APP_KEY=your_app_key
PUSHER_APP_SECRET=your_app_secret
PUSHER_APP_CLUSTER=mt1
```

## 🔧 Configuration

### Broadcast Configuration

The application uses Pusher for real-time message broadcasting. Configure it in `config/broadcasting.php`:

```php
'default' => env('BROADCAST_DRIVER', 'pusher'),

'connections' => [
    'pusher' => [
        'driver' => 'pusher',
        'key' => env('PUSHER_APP_KEY'),
        'secret' => env('PUSHER_APP_SECRET'),
        'app_id' => env('PUSHER_APP_ID'),
        'options' => [
            'cluster' => env('PUSHER_APP_CLUSTER'),
            'useTLS' => true,
        ],
    ],
],
```

### Channel Authorization

Broadcasting channels are authorized in `routes/channels.php`. The chat channel is open to all authenticated users:

```php
Broadcast::channel('chat', function(){
    return true;
});
```

## 📁 Project Structure

```
Laravel-vuejs-realtime-chat/
├── app/
│   ├── User.php                 # User model
│   ├── Http/
│   │   ├── Controllers/
│   │   │   ├── ChatController.php    # Chat logic
│   │   │   ├── HomeController.php    # Dashboard
│   │   │   └── Controller.php
│   │   ├── Kernel.php
│   │   └── Middleware/
│   ├── Events/                  # Application events
│   ├── Listeners/               # Event listeners
│   ├── Providers/               # Service providers
│   └── Console/                 # Console commands
├── resources/
│   ├── assets/
│   │   ├── js/
│   │   │   └── app.js           # Vue.js application entry
│   │   └── sass/
│   │       └── app.scss         # Stylesheet
│   ├── views/
│   │   └── ...                  # Blade templates
│   └── lang/
├── routes/
│   ├── web.php                  # Web routes
│   ├── api.php                  # API routes
│   ├── channels.php             # Broadcasting channels
│   └── console.php
├── config/
│   ├── app.php
│   ├── broadcasting.php         # Broadcasting configuration
│   ├── database.php
│   ├── mail.php
│   └── ...
├── database/
│   ├── migrations/              # Database migrations
│   ├── seeds/                   # Database seeders
│   └── factories/               # Model factories
├── storage/                     # Cache, logs, uploads
├── tests/                       # Test files
├── public/
│   ├── index.php               # Application entry point
│   ├── css/                    # Compiled CSS
│   └── js/                     # Compiled JavaScript
├── webpack.mix.js              # Laravel Mix configuration
├── composer.json               # PHP dependencies
└── package.json                # JavaScript dependencies
```

## 💬 Usage

### 1. Start Development Server

```bash
php artisan serve
```

The application will be available at `http://localhost:8000`

### 2. Compile Frontend Assets

#### Development Mode (with hot reloading):
```bash
npm run hot
```

#### Development Mode (watch):
```bash
npm run watch
```

#### Production Build:
```bash
npm run production
```

### 3. Access the Application

1. Visit `http://localhost:8000`
2. Register a new account or login
3. Navigate to the chat page at `/chat`
4. Send and receive messages in real-time

## 🛠️ Technologies

### Backend
- **Laravel 5.5** - PHP web application framework
- **PHP** >= 7.0.0
- **Pusher** - Real-time messaging service (~3.0)
- **MySQL** - Database

### Frontend
- **Vue.js 2.5** - Progressive JavaScript framework
- **Bootstrap Sass 3.3** - CSS framework
- **Axios** - HTTP client for API calls
- **jQuery 3.2** - JavaScript utilities
- **Lodash 4.17** - Utility library

### Build Tools
- **Laravel Mix** - Webpack wrapper for asset compilation
- **Webpack** - Module bundler
- **Cross-env** - Run scripts cross-platform
- **NPM** - JavaScript package manager

## 👨‍💻 Development

### Running Tests

```bash
php artisan test
# or for older Laravel versions:
php vendor/bin/phpunit
```

### Database Seeding

```bash
php artisan db:seed
```

### Create New Migration

```bash
php artisan make:migration migration_name
```

### Create New Model

```bash
php artisan make:model ModelName -m
```

### Laravel Tinker (REPL)

```bash
php artisan tinker
```

## 📡 API Endpoints

### Web Routes

| Method | Route | Controller | Description |
|--------|-------|------------|-------------|
| GET | `/` | welcome | Welcome page |
| GET | `/chat` | ChatController@chat | Chat interface |
| POST | `/send` | ChatController@send | Send message |
| GET | `/home` | HomeController@index | User dashboard |

### Authentication Routes

- `GET /login` - Login page
- `POST /login` - Submit login
- `POST /logout` - Logout user
- `GET /register` - Registration page
- `POST /register` - Submit registration
- `POST /password/email` - Password reset request
- `POST /password/reset` - Reset password

### API Routes

| Method | Route | Description |
|--------|-------|-------------|
| GET | `/api/user` | Get authenticated user (requires auth:api middleware) |

## 📤 Broadcasting

The application uses Pusher for real-time communication. Messages are broadcast to the `chat` channel when sent.

### Broadcasting Channels (routes/channels.php)

- **chat** - Public chat channel for all authenticated users
- **App.User.{id}** - Private user channel for notifications

## 📝 Notes

- Ensure Pusher credentials are properly configured in `.env`
- The application requires a live internet connection for real-time features
- Use `npm run production` before deploying to production
- Database migrations run automatically in `composer.json` post-install script

## 📄 License

This project is licensed under the MIT License. See the LICENSE file for more details.

## 👥 Contributing

Contributions are welcome! Please feel free to submit pull requests or open issues for bugs and feature requests.

## 🆘 Troubleshooting

### Messages not appearing in real-time?
- Check Pusher credentials in `.env`
- Verify Pusher service status
- Check browser console for JavaScript errors
- Ensure Broadcast driver is set to 'pusher'

### Database connection errors?
- Verify MySQL is running
- Check database credentials in `.env`
- Run `php artisan migrate`

### Assets not loading?
- Run `npm install`
- Run `npm run development` or `npm run watch`
- Clear browser cache (Ctrl+Shift+Delete)

## 📧 Support

For issues and questions, please create an issue in the repository or contact the development team.

---

**Happy Chatting! 💬**
