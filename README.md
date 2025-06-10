Cours Complet Laravel
Ce cours est conçu pour vous emmener des bases de Laravel aux concepts plus avancés, vous permettant de construire des applications web robustes et modernes.

Table des Matières
Introduction à Laravel
Installation et Configuration
Les Fondamentaux de Laravel
Routing
Contrôleurs
Vues (Blade)
Modèles et Eloquent ORM
Migrations de Base de Données
Seeding de Base de Données
Gestion des Formulaires et Validation
Authentification et Autorisation
Les Services
Service Container
Service Providers
Facades
Gestion des Sessions et Cookies
Les Middlwares
Fichiers Statiques (Assets)
Files d'Attente (Queues)
Événements et Listeners
Tests
API RESTful avec Laravel
Déploiement
Ressources Supplémentaires
1. Introduction à Laravel
Laravel est un framework d'application web basé sur PHP, élégant et expressif, conçu pour les artisans du web. Il simplifie de nombreuses tâches courantes utilisées dans le développement web, telles que l'authentification, le routage, les sessions et la mise en cache. Laravel vise à rendre le processus de développement plus agréable pour le développeur sans sacrifier la fonctionnalité des applications.

Pourquoi Laravel ?

Syntaxe Expressive : Code propre et facile à lire.
Écosystème Riche : Nombreux packages officiels et communautaires.
Artisan CLI : Outil en ligne de commande puissant pour la génération de code et la gestion de l'application.
Eloquent ORM : Cartographie objet-relationnel intuitive pour interagir avec les bases de données.
Blade Templating Engine : Moteur de templating simple mais puissant.
Communauté Active : Large support et documentation complète.
2. Installation et Configuration
Pour commencer avec Laravel, vous avez besoin de :

PHP (version 8.2+ recommandée)
Composer (gestionnaire de dépendances pour PHP)
Un serveur web (Apache ou Nginx)
Une base de données (MySQL, PostgreSQL, SQLite, SQL Server)
Installation de Composer

Téléchargez et installez Composer depuis getcomposer.org.

Création d'un nouveau projet Laravel

Ouvrez votre terminal et exécutez la commande suivante :

Bash
composer create-project laravel/laravel mon-application-laravel
Remplacez mon-application-laravel par le nom de votre projet.

Démarrage du serveur de développement

Laravel inclut un serveur de développement local pratique :

Bash
cd mon-application-laravel
php artisan serve
Votre application sera accessible à l'adresse http://127.0.0.1:8000.

Configuration de la base de données

Ouvrez le fichier .env à la racine de votre projet. Configurez les informations de votre base de données :

Extrait de code
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=nom_de_votre_base
DB_USERNAME=votre_utilisateur
DB_PASSWORD=votre_mot_de_passe
3. Les Fondamentaux de Laravel
3.1. Routing

Le routage est le processus par lequel Laravel détermine quelle action doit être exécutée lorsqu'une requête HTTP arrive à une URL spécifique.

Les routes sont définies dans les fichiers du dossier routes/:

web.php : Routes pour les applications web (avec gestion de session, CSRF, etc.).
api.php : Routes pour les API sans état.
console.php : Routes pour les commandes Artisan.
channels.php : Routes pour le broadcasting d'événements.
Exemples de routes dans routes/web.php :

PHP
<?php

use Illuminate\Support\Facades\Route;

Route::get('/', function () {
    return view('welcome');
});

Route::get('/salut', function () {
    return 'Bonjour le monde !';
});

Route::get('/utilisateurs/{id}', function ($id) {
    return 'Utilisateur ID: ' . $id;
});

// Route avec un nom
Route::get('/contact', function () {
    return 'Page de contact';
})->name('contact');

// Redirection
Route::redirect('/accueil', '/');
Récupérer une URL par son nom :

PHP
// Dans une vue ou un contrôleur
echo route('contact');
3.2. Contrôleurs

Les contrôleurs regroupent la logique de traitement des requêtes HTTP. Ils sont stockés dans le répertoire app/Http/Controllers.

Créer un contrôleur :

Bash
php artisan make:controller MonController
Exemple de app/Http/Controllers/MonController.php :

PHP
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class MonController extends Controller
{
    public function index()
    {
        return 'Bienvenue sur la page d\'accueil via un contrôleur !';
    }

    public function show($id)
    {
        return 'Affichage de l\'élément numéro : ' . $id;
    }
}
Lier des routes à des contrôleurs :

PHP
// Dans routes/web.php
use App\Http\Controllers\MonController;

Route::get('/accueil-controller', [MonController::class, 'index']);
Route::get('/elements/{id}', [MonController::class, 'show']);
3.3. Vues (Blade)

Blade est le moteur de templating simple et puissant de Laravel. Les fichiers Blade sont stockés dans le répertoire resources/views et ont une extension .blade.php.

Exemple de resources/views/greeting.blade.php :

HTML
<!DOCTYPE html>
<html>
<head>
    <title>Salut Blade</title>
</head>
<body>
    <h1>Bonjour, {{ $nom }} !</h1>

    @if ($age >= 18)
        <p>Vous êtes majeur.</p>
    @else
        <p>Vous êtes mineur.</p>
    @endif

    <ul>
        @foreach ($fruits as $fruit)
            <li>{{ $fruit }}</li>
        @endforeach
    </ul>

    @include('partials.footer')
</body>
</html>
Passer des données à une vue :

PHP
// Dans une route ou un contrôleur
Route::get('/salut/{nom}', function ($nom) {
    $fruits = ['pomme', 'banane', 'orange'];
    return view('greeting', [
        'nom' => $nom,
        'age' => 25,
        'fruits' => $fruits
    ]);
});
Héritage de vues (Layouts) :

resources/views/layouts/app.blade.php :

HTML
<!DOCTYPE html>
<html>
<head>
    <title>@yield('title', 'Mon Application')</title>
</head>
<body>
    <header>
        <h1>Mon Application</h1>
    </header>

    <main>
        @yield('content')
    </main>

    <footer>
        <p>&copy; 2024 Mon Application</p>
    </footer>
</body>
</html>
resources/views/child.blade.php :

HTML
@extends('layouts.app')

@section('title', 'Page Enfant')

@section('content')
    <p>Ceci est le contenu de la page enfant.</p>
@endsection
3.4. Modèles et Eloquent ORM

Eloquent est l'ORM (Object-Relational Mapper) de Laravel. Il fournit une implémentation ActiveRecord élégante pour interagir avec votre base de données. Chaque table de base de données a un "Modèle" correspondant qui est utilisé pour interagir avec cette table.

Créer un modèle :

Bash
php artisan make:model Post -m
L'option -m crée également une migration pour la table posts.

Exemple de app/Models/Post.php :

PHP
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Post extends Model
{
    use HasFactory;

    // Définir les attributs qui peuvent être assignés en masse
    protected $fillable = [
        'title',
        'content',
        'user_id'
    ];

    // Définir les relations (ex: un Post appartient à un User)
    public function user()
    {
        return $this->belongsTo(User::class);
    }
}
 Interagir avec la base de données (exemples dans un contrôleur) :

PHP
<?php

namespace App\Http\Controllers;

use App\Models\Post;
use Illuminate\Http\Request;

class PostController extends Controller
{
    public function index()
    {
        $posts = Post::all(); // Récupérer tous les posts
        return view('posts.index', compact('posts'));
    }

    public function show($id)
    {
        $post = Post::findOrFail($id); // Récupérer un post par ID, ou 404
        return view('posts.show', compact('post'));
    }

    public function store(Request $request)
    {
        $post = Post::create([
            'title' => $request->input('title'),
            'content' => $request->input('content'),
            'user_id' => auth()->id(), // Exemple : associer au user connecté
        ]);
        return redirect('/posts/' . $post->id)->with('success', 'Post créé !');
    }

    public function update(Request $request, $id)
    {
        $post = Post::findOrFail($id);
        $post->update($request->all());
        return redirect('/posts/' . $post->id)->with('success', 'Post mis à jour !');
    }

    public function destroy($id)
    {
        $post = Post::findOrFail($id);
        $post->delete();
        return redirect('/posts')->with('success', 'Post supprimé !');
    }
}
3.5. Migrations de Base de Données

 Les migrations sont comme le contrôle de version pour votre base de données. Elles vous permettent de modifier et de partager le schéma de la base de données de votre application de manière facile et structurée.

Les fichiers de migration sont stockés dans le répertoire database/migrations.

Créer une migration (ex: pour une table products) :

Bash
php artisan make:migration create_products_table
Exemple de migration pour create_products_table :

PHP
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    /**
     * Run the migrations.
     */
    public function up(): void
    {
        Schema::create('products', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->text('description')->nullable();
            $table->decimal('price', 8, 2);
            $table->integer('stock')->default(0);
            $table->timestamps(); // created_at et updated_at
        });
    }

    /**
     * Reverse the migrations.
     */
    public function down(): void
    {
        Schema::dropIfExists('products');
    }
};
Exécuter les migrations :

Bash
php artisan migrate
Annuler la dernière migration :

Bash
php artisan migrate:rollback
Réinitialiser toutes les migrations :

Bash
php artisan migrate:fresh # Supprime toutes les tables et exécute les migrations
3.6. Seeding de Base de Données

Les "seeders" vous permettent de remplir votre base de données avec des données de test ou des données initiales.

Les fichiers de seeder sont stockés dans le répertoire database/seeders.

Créer un seeder :

Bash
php artisan make:seeder ProductSeeder
Exemple de database/seeders/ProductSeeder.php :

PHP
<?php

namespace Database\Seeders;

use Illuminate\Database\Console\Seeds\WithoutModelEvents;
use Illuminate\Database\Seeder;
use App\Models\Product; // N'oubliez pas d'importer le modèle

class ProductSeeder extends Seeder
{
    /**
     * Run the database seeds.
     */
    public function run(): void
    {
        Product::create([
            'name' => 'Ordinateur Portable',
            'description' => 'Un puissant ordinateur portable pour le développement.',
            'price' => 1200.00,
            'stock' => 10
        ]);

        Product::create([
            'name' => 'Smartphone Pro',
            'description' => 'Le dernier smartphone avec des fonctionnalités avancées.',
            'price' => 800.00,
            'stock' => 25
        ]);
    }
}
 Exécuter un seeder spécifique :

Bash
php artisan db:seed --class=ProductSeeder
 Exécuter tous les seeders (définis dans DatabaseSeeder.php) :

Bash
php artisan db:seed
Pour exécuter tous les seeders, vous devez les appeler depuis database/seeders/DatabaseSeeder.php :

PHP
<?php

namespace Database\Seeders;

use Illuminate\Database\Seeder;

class DatabaseSeeder extends Seeder
{
    /**
     * Seed the application's database.
     */
    public function run(): void
    {
        $this->call([
            ProductSeeder::class,
            // Autres seeders ici...
        ]);
    }
}
4. Gestion des Formulaires et Validation
 Laravel facilite la gestion des requêtes HTTP et la validation des données envoyées via les formulaires.

Accéder aux données de la requête

 Utilisez l'objet Illuminate\Http\Request.

PHP
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class FormController extends Controller
{
    public function processForm(Request $request)
    {
        $name = $request->input('name'); // Accéder à un champ spécifique
        $email = $request->email;       // Alternative plus courte
        $allData = $request->all();     // Récupérer toutes les données
        $onlyNameEmail = $request->only(['name', 'email']); // Récupérer seulement certains champs

        return "Nom: {$name}, Email: {$email}";
    }
}
Validation des données

Laravel offre un système de validation puissant.

```php
<?php

 namespace App\Http\Controllers;

 use Illuminate\Http\Request;

 class FormController extends Controller
{
public function submit(Request $request)
{
$validated = $request->validate([
'name' => 'required|max:255',
'email' => 'required|email|unique:users',
'password' => 'required|min:8|confirmed',
]);

    // Les données sont valides et accessibles via $validated
    // ou $request->input('name') etc.

    return back()->with('success', 'Formulaire soumis avec succès !');
}
}


**Afficher les erreurs de validation dans les vues Blade :**

```html
@if ($errors->any())
    <div class="alert alert-danger">
        <ul>
            @foreach ($errors->all() as $error)
                <li>{{ $error }}</li>
            @endforeach
        </ul>
    </div>
@endif

<form method="POST" action="/submit-form">
    @csrf <label for="name">Nom:</label>
    <input type="text" name="name" value="{{ old('name') }}">
    @error('name')
        <div class="alert alert-danger">{{ $message }}</div>
    @enderror

    <label for="email">Email:</label>
    <input type="email" name="email" value="{{ old('email') }}">
    @error('email')
        <div class="alert alert-danger">{{ $message }}</div>
    @enderror

    <button type="submit">Envoyer</button>
</form>
old('name') est utile pour re-remplir les champs après une erreur de validation.

5. Authentification et Autorisation
Laravel fournit un système d'authentification robuste et facile à utiliser.

Authentification

Laravel Breeze ou Jetstream sont d'excellents packages pour démarrer rapidement avec l'authentification.

Installation de Breeze (pour un projet simple) :

Bash
composer require laravel/breeze --dev
php artisan breeze:install
php artisan migrate
npm install && npm run dev
Ceci installera les vues, les routes et les contrôleurs nécessaires pour l'enregistrement, la connexion, la réinitialisation de mot de passe, etc.

Accéder à l'utilisateur authentifié :

PHP
// Dans un contrôleur
use Illuminate\Http\Request;

public function dashboard(Request $request)
{
    $user = $request->user(); // L'utilisateur actuellement authentifié
    // Ou
    $user = auth()->user();

    return view('dashboard', compact('user'));
}
Protéger les routes avec le middleware auth :

PHP
// Dans routes/web.php
Route::middleware(['auth'])->group(function () {
    Route::get('/dashboard', function () {
        return view('dashboard');
    });
    Route::resource('posts', PostController::class); // Protège toutes les actions CRUD
});
Autorisation

Laravel fournit des moyens simples d'organiser la logique d'autorisation de votre application.

Gates (Portes) : Pour des autorisations simples et rapides.

 Dans AuthServiceProvider.php (méthode boot) :

PHP
use Illuminate\Support\Facades\Gate;
use App\Models\User;
use App\Models\Post;

public function boot(): void
{
    Gate::define('update-post', function (User $user, Post $post) {
        return $user->id === $post->user_id;
    });
}
 Utiliser un Gate :

PHP
// Dans un contrôleur
if (Gate::allows('update-post', $post)) {
    // L'utilisateur peut mettre à jour le post
}

if (Gate::denies('update-post', $post)) {
    // L'utilisateur ne peut pas mettre à jour le post
}

// Ou avec le middleware 'can'
Route::put('/posts/{post}', [PostController::class, 'update'])->middleware('can:update-post,post');
Policies (Politiques) : Pour regrouper la logique d'autorisation par modèle.

Créer une politique :

Bash
php artisan make:policy PostPolicy --model=Post
Exemple de app/Policies/PostPolicy.php :

PHP
<?php

namespace App\Policies;

use App\Models\User;
use App\Models\Post;
use Illuminate\Auth\Access\Response;

class PostPolicy
{
    /**
     * Determine whether the user can update the model.
     */
    public function update(User $user, Post $post): bool
    {
        return $user->id === $post->user_id;
    }

    /**
     * Determine whether the user can delete the model.
     */
    public function delete(User $user, Post $post): bool
    {
        return $user->id === $post->user_id;
    }
}
Enregistrer la politique dans AuthServiceProvider.php :

PHP
protected $policies = [
    Post::class => PostPolicy::class,
];
Utiliser une politique :

PHP
// Dans un contrôleur
use App\Models\Post;

public function update(Request $request, Post $post)
{
    $this->authorize('update', $post); // Ou $request->user()->can('update', $post);

    // Mettre à jour le post
}

// Dans une vue Blade
@can('update', $post)
    <a href="/posts/{{ $post->id }}/edit">Modifier</a>
@endcan
6. Les Services
6.1. Service Container (Conteneur de Services)

Le conteneur de services de Laravel est un outil puissant pour la gestion des dépendances et l'injection de dépendances. Il vous permet de gérer les dépendances de vos classes et d'injecter celles-ci de manière automatique.

Exemple d'injection de dépendances :

PHP
<?php

namespace App\Http\Controllers;

use App\Services\OrderService; // Supposons que vous avez un service d'ordre

class OrderController extends Controller
{
    protected $orderService;

    // Le conteneur injecte automatiquement OrderService
    public function __construct(OrderService $orderService)
    {
        $this->orderService = $orderService;
    }

    public function processOrder()
    {
        $this->orderService->placeOrder(...);
        // ...
    }
}
6.2. Service Providers (Fournisseurs de Services)

Les fournisseurs de services sont le point central de bootstaping de toutes les applications Laravel. Votre propre application, ainsi que tous les services de Laravel, sont bootstrapped via des fournisseurs de services.

Les fournisseurs de services sont stockés dans app/Providers.

Enregistrer un service dans le conteneur :

Dans un AppServiceProvider.php ou un nouveau Service Provider :

PHP
<?php

namespace App\Providers;

use Illuminate\Support\ServiceProvider;
use App\Services\Contracts\PaymentGateway; // Une interface
use App\Services\StripePaymentGateway; // Une implémentation

class AppServiceProvider extends ServiceProvider
{
    /**
     * Register any application services.
     */
    public function register(): void
    {
        // Bind une interface à une implémentation concrète
        $this->app->bind(PaymentGateway::class, StripePaymentGateway::class);

        // Bind un singleton (une seule instance sera créée)
        $this->app->singleton(OrderService::class, function ($app) {
            return new OrderService($app->make(PaymentGateway::class));
        });
    }

    /**
     * Bootstrap any application services.
     */
    public function boot(): void
    {
        //
    }
}
6.3. Facades

Les Facades fournissent une interface "statique" aux classes disponibles dans le conteneur de services.

Exemples courants : Route, DB, Cache, Storage, Auth.

PHP
use Illuminate\Support\Facades\Cache;

Cache::put('key', 'value', $minutes);
Bien que les Facades donnent l'impression d'être statiques, ce sont des proxies vers les instances du conteneur de services, permettant des tests plus faciles et une meilleure gestion des dépendances.

7. Gestion des Sessions et Cookies
Sessions

Laravel fournit une API expressive pour gérer les sessions utilisateur via différentes backends (fichier, base de données, Redis, Memcached).

Accéder à la session :

PHP
// Via la requête
use Illuminate\Http\Request;

public function myMethod(Request $request)
{
    $value = $request->session()->get('key');
    $request->session()->put('key', 'value');
    $request->session()->flash('status', 'Profil mis à jour !'); // Donnée flash (disponible pour la prochaine requête)
}

// Via l'aide globale 'session()'
session(['key' => 'value']);
$value = session('key');
Afficher les messages flash dans Blade :

Extrait de code
@if (session('status'))
    <div class="alert alert-success">
        {{ session('status') }}
    </div>
@endif
Cookies

Laravel offre également un moyen facile de gérer les cookies.

PHP
use Illuminate\Http\Request;
use Illuminate\Http\Response;

// Récupérer un cookie
public function getCookie(Request $request)
{
    $value = $request->cookie('name');
    return $value;
}

// Définir un cookie
public function setCookie()
{
    $response = new Response('Bonjour');
    $response->cookie('name', 'value', $minutes = 60); // Nom, valeur, durée en minutes
    return $response;
}
8. Les Middlewares
Les middlewares agissent comme des filtres HTTP pour les requêtes entrantes vers votre application. Par exemple, Laravel inclut un middleware qui vérifie si l'utilisateur de votre application est authentifié. Si l'utilisateur n'est pas authentifié, le middleware redirigera l'utilisateur vers l'écran de connexion.

Les middlewares sont stockés dans app/Http/Middleware.

Créer un middleware :

Bash
php artisan make:middleware CheckAge
Exemple de app/Http/Middleware/CheckAge.php :

PHP
<?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;
use Symfony\Component\HttpFoundation\Response;

class CheckAge
{
    /**
     * Handle an incoming request.
     *
     * @param  \Closure(\Illuminate\Http\Request): (\Symfony\Component\HttpFoundation\Response)  $next
     */
    public function handle(Request $request, Closure $next): Response
    {
        if ($request->age < 18) {
            return redirect('home'); // Redirige si l'âge est inférieur à 18
        }

        return $next($request); // Passe la requête au prochain middleware/route
    }
}
Enregistrer un middleware :

Dans app/Http/Kernel.php :

Middleware global : Appliqué à chaque requête HTTP.
PHP
protected $middleware = [
    // \App\Http\Middleware\TrustProxies::class,
    // ...
    \App\Http\Middleware\CheckAge::class, // Ajout ici
];
Middleware de groupe : Appliqué à un groupe de routes.
PHP
protected $middlewareGroups = [
    'web' => [
        // ...
        \App\Http\Middleware\CheckAge::class, // Ou ici pour le groupe 'web'
    ],
    'api' => [
        // ...
    ],
];
Middleware de route : Appliqué à des routes spécifiques.
PHP
protected $routeMiddleware = [
    // ...
    'age' => \App\Http\Middleware\CheckAge::class,
];
 Utiliser un middleware de route :

PHP
// Dans routes/web.php
Route::get('/profile', function () {
    // ...
})->middleware('age');

// Avec un paramètre
Route::get('/admin', function () {
    // ...
})->middleware('can:manage-users');
9. Fichiers Statiques (Assets)
Laravel ne gère pas directement les assets (CSS, JS, images) par défaut, mais il fournit des outils et des structures pour les organiser.

Dossier public/ : C'est le seul dossier accessible directement via HTTP. Tous vos assets compilés ou publics y seront placés.
Dossier resources/ : Contient vos assets bruts (Sass, Less, JavaScript ES6) qui seront compilés.
Mix (Laravel Mix)

Laravel Mix est une API fluide pour définir des étapes de compilation Webpack pour votre application Laravel en utilisant plusieurs préprocesseurs CSS et JavaScript courants.

Installation :

Bash
npm install
Configuration (fichier webpack.mix.js à la racine du projet) :

JavaScript
const mix = require('laravel-mix');

mix.js('resources/js/app.js', 'public/js')
   .postCss('resources/css/app.css', 'public/css', [
       //
   ]);

// Si vous utilisez Sass
// mix.sass('resources/scss/app.scss', 'public/css');

// Pour des fichiers multiples
// mix.js('resources/js/admin.js', 'public/js')
//    .js('resources/js/frontend.js', 'public/js');
Compilation :

Bash
npm run dev      // Compilation pour le développement
npm run watch    // Recompile automatiquement lors des changements
npm run prod     // Compilation optimisée pour la production
Utiliser les assets dans Blade :

Utilisez la fonction mix() pour inclure les assets compilés, ce qui gère automatiquement la mise en cache (cache busting) en ajoutant un hachage au nom du fichier.

HTML
<link href="{{ mix('css/app.css') }}" rel="stylesheet">
<script src="{{ mix('js/app.js') }}"></script>
10. Files d'Attente (Queues)
 Les files d'attente vous permettent de décharger des tâches qui prennent beaucoup de temps, comme l'envoi d'e-mails, le traitement d'images ou la génération de rapports, afin que votre application web puisse répondre rapidement aux requêtes.

 Laravel fournit une API unifiée pour les différentes backends de files d'attente (base de données, Redis, SQS, Beanstalkd).

Configuration :

Dans .env :

Extrait de code
QUEUE_CONNECTION=database # Ou redis, sqs, sync (pour le développement)
Créer la table des jobs (si QUEUE_CONNECTION=database) :

Bash
php artisan queue:table
php artisan migrate
Créer un job :

Bash
php artisan make:job ProcessPodcast
Exemple de app/Jobs/ProcessPodcast.php :

PHP
<?php

namespace App\Jobs;

use App\Models\Podcast;
use Illuminate\Bus\Queueable;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Foundation\Bus\Dispatchable;
use Illuminate\Queue\InteractsWithQueue;
use Illuminate\Queue\SerializesModels;

class ProcessPodcast implements ShouldQueue
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels;

    protected $podcast;

    /**
     * Create a new job instance.
     */
    public function __construct(Podcast $podcast)
    {
        $this->podcast = $podcast;
    }

    /**
     * Execute the job.
     */
    public function handle(): void
    {
        // Logique complexe de traitement du podcast
        logger('Traitement du podcast : ' . $this->podcast->title);
        // Ex: redimensionner des images, envoyer une notification, etc.
    }
}

Dispatcher un job :

PHP
use App\Jobs\ProcessPodcast;
use App\Models\Podcast;

// ... dans un contrôleur ou un service
$podcast = Podcast::find(1);
ProcessPodcast::dispatch($podcast); // Le job sera mis en file d'attente
 Exécuter le worker de la file d'attente :

 Pour que les jobs soient traités, vous devez exécuter un "worker" de file d'attente.

Bash
php artisan queue:work
Pour la production, utilisez un processus manager comme Supervisor pour maintenir le worker en fonctionnement.

11. Événements et Listeners
 Les événements de Laravel fournissent une implémentation simple de l'observateur, vous permettant de souscrire et d'écouter divers événements qui se produisent dans votre application.

Créer un événement et un listener :

Bash
php artisan make:event OrderShipped
php artisan make:listener SendShipmentNotification --event=OrderShipped
Exemple de app/Events/OrderShipped.php :

PHP
<?php

namespace App\Events;

use App\Models\Order;
use Illuminate\Broadcasting\InteractsWithSockets;
use Illuminate\Foundation\Events\Dispatchable;
use Illuminate\Queue\SerializesModels;

class OrderShipped
{
    use Dispatchable, InteractsWithSockets, SerializesModels;

    public $order;

    /**
     * Create a new event instance.
     */
    public function __construct(Order $order)
    {
        $this->order = $order;
    }
}
Exemple de app/Listeners/SendShipmentNotification.php :

PHP
<?php

namespace App\Listeners;

use App\Events\OrderShipped;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Queue\InteractsWithQueue;

class SendShipmentNotification implements ShouldQueue // Peut être mis en file d'attente
{
    /**
     * Create the event listener.
     */
    public function __construct()
    {
        //
    }

    /**
     * Handle the event.
     */
    public function handle(OrderShipped $event): void
    {
        // Envoyer une notification par email, SMS, etc.
        logger('Commande expédiée : ' . $event->order->id);
    }
}
Enregistrer les événements et les listeners :

Dans app/Providers/EventServiceProvider.php :

PHP
protected $listen = [
    OrderShipped::class => [
        SendShipmentNotification::class,
    ],
];
Dispatcher un événement :

PHP
use App\Events\OrderShipped;
use App\Models\Order;

// ... dans un contrôleur, un service, etc.
$order = Order::find(1);
event(new OrderShipped($order)); // Dispatche l'événement
12. Tests
Laravel est livré avec le support des tests PHPUnit "out-of-the-box". Vous pouvez exécuter les tests depuis votre terminal.

 Les tests sont stockés dans le répertoire tests/.

tests/Feature : Tests qui sollicitent des parties plus importantes de votre application (ex: interactions avec la base de données, requêtes HTTP).
tests/Unit : Tests isolés d'unités de code (ex: une fonction spécifique d'une classe).
Exécuter les tests :

Bash
php artisan test
Créer un test feature :

Bash
php artisan make:test UserRegistrationTest
Exemple de tests/Feature/UserRegistrationTest.php :

PHP
<?php

namespace Tests\Feature;

use Illuminate\Foundation\Testing\RefreshDatabase;
use Illuminate\Foundation\Testing\WithFaker;
use Tests\TestCase;
use App\Models\User;

class UserRegistrationTest extends TestCase
{
    use RefreshDatabase; // Réinitialise la base de données pour chaque test

    /**
     * A basic feature test example.
     */
    public function test_new_users_can_register(): void
    {
        $response = $this->post('/register', [
            'name' => 'Test User',
            'email' => 'test@example.com',
            'password' => 'password',
            'password_confirmation' => 'password',
        ]);

        $response->assertRedirect('/dashboard'); // Vérifie la redirection
        $this->assertDatabaseHas('users', ['email' => 'test@example.com']); // Vérifie l'existence en DB
        $this->assertAuthenticated(); // Vérifie que l'utilisateur est authentifié
    }

    public function test_registration_screen_can_be_rendered(): void
    {
        $response = $this->get('/register');
        $response->assertStatus(200); // Vérifie le statut HTTP
    }
}
13. API RESTful avec Laravel
Laravel facilite la création d'APIs RESTful.

Routes API

Les routes pour votre API sont définies dans routes/api.php. Par défaut, toutes les routes dans ce fichier sont préfixées par /api et n'ont pas de protection CSRF ou de gestion de session.

PHP
// routes/api.php
use App\Http\Controllers\Api\PostController; // Créez un dossier Api pour vos contrôleurs d'API

Route::get('/posts', [PostController::class, 'index']);
Route::post('/posts', [PostController::class, 'store']);
Route::get('/posts/{post}', [PostController::class, 'show']);
Route::put('/posts/{post}', [PostController::class, 'update']);
Route::delete('/posts/{post}', [PostController::class, 'destroy']);
Ressources API

Les ressources API (Resource Classes) vous permettent de transformer vos modèles Eloquent et leurs collections en JSON de manière simple et personnalisable.

Créer une ressource :

Bash
php artisan make:resource PostResource
php artisan make:resource PostCollection # Pour une collection de posts
 Exemple de app/Http/Resources/PostResource.php :

PHP
<?php

namespace App\Http\Resources;

use Illuminate\Http\Request;
use Illuminate\Http\Resources\Json\JsonResource;

class PostResource extends JsonResource
{
    /**
     * Transform the resource into an array.
     *
     * @return array<string, mixed>
     */
    public function toArray(Request $request): array
    {
        return [
            'id' => $this->id,
            'title' => $this->title,
            'content' => $this->content,
            'published_at' => $this->created_at->format('d/m/Y H:i:s'),
            'author' => new UserResource($this->whenLoaded('user')), // Chargement conditionnel d'une relation
            'links' => [
                'self' => route('posts.show', $this->id),
            ],
        ];
    }
}
Utiliser la ressource dans un contrôleur API :

PHP
<?php

namespace App\Http\Controllers\Api;

use App\Http\Controllers\Controller;
use App\Http\Resources\PostResource;
use App\Models\Post;
use Illuminate\Http\Request;

class PostController extends Controller
{
    public function index()
    {
        return PostResource::collection(Post::paginate()); // Pour une collection paginée
    }

    public function show(Post $post)
    {
        return new PostResource($post);
    }
}
Authentification API (Sanctum)

Laravel Sanctum offre un système d'authentification simple pour les SPAs, les applications mobiles et les API basées sur les tokens.

Installation :

Bash
composer require laravel/sanctum
php artisan vendor:publish --provider="Laravel\Sanctum\SanctumServiceProvider"
php artisan migrate
Générer des tokens (pour les applications externes) :

PHP
// Dans un contrôleur de connexion par exemple
use Illuminate\Http\Request;

public function login(Request $request)
{
    $credentials = $request->validate([
        'email' => ['required', 'email'],
        'password' => ['required'],
    ]);

    if (Auth::attempt($credentials)) {
        $user = Auth::user();
        $token = $user->createToken('my-app-token')->plainTextToken; // Créer un token
        return response()->json(['token' => $token]);
    }

    return response()->json(['message' => 'Identifiants invalides'], 401);
}
Protéger les routes API avec Sanctum :

PHP
// routes/api.php
Route::middleware('auth:sanctum')->get('/user', function (Request $request) {
    return $request->user();
});
Les requêtes client doivent inclure le token dans l'en-tête Authorization: Bearer YOUR_TOKEN.

14. Déploiement
Le déploiement d'une application Laravel implique plusieurs étapes :

Configuration de l'environnement de production :
Mettre APP_ENV=production et APP_DEBUG=false dans votre fichier .env.
Configurer les variables d'environnement sensibles.
Générer la clé d'application si ce n'est pas déjà fait : php artisan key:generate.
Mise à jour des dépendances :
Exécuter composer install --optimize-autoloader --no-dev.
Exécuter npm install && npm run prod pour compiler les assets en production.
Permissions de fichiers :
Assurez-vous que les répertoires storage et bootstrap/cache sont inscriptibles par le serveur web. <!-- end list -->
Extrait de code
chmod -R 775 storage
chmod -R 775 bootstrap/cache
chown -R www-data:www-data storage bootstrap/cache # Ou l'utilisateur de votre serveur web
Optimisation de l'application :
Cache de configuration : php artisan config:cache
Cache des routes : php artisan route:cache
Cache des vues : php artisan view:cache
Exécuter les migrations :
php artisan migrate --force (nécessite --force en production)
Configuration du serveur web (Nginx / Apache) :
Le document root de votre serveur doit pointer vers le répertoire public de votre application Laravel.
Exemple Nginx (extrait) :
Nginx
server {
    listen 80;
    server_name yourdomain.com;
    root /var/www/yourdomain.com/public; # Votre chemin vers le dossier public

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Content-Type-Options "nosniff";

    index index.php index.html index.htm;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php8.2-fpm.sock; # Vérifiez votre version PHP
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.(env|git|svn) {
        deny all;
    }
}
Configuration des files d'attente :
Mettre en place un processus de surveillance comme Supervisor pour exécuter php artisan queue:work en permanence.
Monitoring et logs :
Surveillez les logs de votre application (storage/logs/laravel.log).
Outils de déploiement simplifiés :

Laravel Forge : Automatise le provisionnement et le déploiement de serveurs.
Laravel Envoyer : Pour le déploiement sans temps d'arrêt.
Capistrano / Deployer : Outils de déploiement personnalisés.
15. Ressources Supplémentaires
Documentation Officielle Laravel : https://laravel.com/docs (Indispensable !)
Laracasts : https://laracasts.com/ (Tutoriels vidéo de haute qualité)
Awesome Laravel : Une liste curatée de packages, ressources et autres choses géniales sur Laravel.
Stack Overflow : Pour les questions et problèmes spécifiques.
GitHub : Explorez d'autres projets Laravel open source.
