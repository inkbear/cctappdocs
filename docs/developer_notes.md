# Developer Notes for CCT App

## Web Server and Database Installation for localhost (Mac OS)

* Install XAMPP (without VM!)
* Change doc root to /Users/paulcoogan/Documents/www/cctapp/public
* Use the PHPMyAdmin installed as part of XAMPP to create the MySQL database
cctapp as UTF8 MB4 ci

## Install Laravel

* Install Laravel - refer to site for instructions
    * laravel new --auth cctapp (creates directory and all files)
    * Update .env for database name 
    * Note that composer was not used but may be:

            composer create-project laravel/laravel cctapp "7.*" --prefer-dist

    * File permissions may need to be modified ot allow the system to read/write files under storage
    
            chmod -R 777 storage

## Install Voyager

* Install Voyager at command line

        composer require tcg/voyager
        php artisan voyager:install

    * If this error occurs follow the instructions in the message:

            SQLSTATE[42000]: Syntax error or access violation: 1071 Specified key was too long; max key length is 767 bytes (SQL: alter table `users` add unique `users_email_unique`(`email`))

    * Edit AppServiceProvider.php

            ...
            public function boot()
            {
                // Added to support older MySQL
                Schema::defaultStringLength(191); (ADDED)
            }
            ...

        * Be sure to delete any tables created on failed runs before rerunning

* Create the admin account in Voyager.

        php artisan voyager:admin paul@inkbear.com --create

* Not sure why this is here but do it anyway.

        php artisan vendor:publish --provider="TCG\Voyager\VoyagerServiceProvider"
        php artisan vendor:publish --provider="Intervention\Image\ImageServiceProviderLaravelRecent"

## Install Voyager Front End

__BACKUP BEFORE DOING THIS. FILES ARE OVERWRITTEN!__

* Require the Package in your fresh Laravel/Voyager project

        composer require pvtl/voyager-frontend

* Run the Installer

        composer dump-autoload && php artisan voyager-frontend:install

* Build the front-end theme assets

        npm install
        npm run dev

## Configure Laravel 

__Backup frequently and check permissions on the storage directory as you go.__

* Set the Laravel search driver in your .env

        echo "SCOUT_DRIVER=tntsearch" >> .env

* Logout route is dorked. Add this to web.php

        Route::get('logout', 'Auth\LoginController@logout', function () {
            return abort(404);
        });

* Cron Entry does nto seemt to be needed (not done yet).

        * * * * * php /path-to-your-project/artisan schedule:run >> /dev/null 2>&1

* Apply template overrides and change look for login:
    * Create the directory resources/views/vendor/voyager-frontend
    * Installation of voyager frontend will set up / 
    * Use /admin for all "work"
    * Update to login look in admin > settings
    
* Modify VoyagerAdminMiddleware.php to have login with no role redirect to /home.

## Add Ranks to Roles 

Maintain hierarchical structure by adding a rank to roles in the roles table as an INT. This allows the roles to be 
additive in this order:

1. admin = 0
1. owner = 10
1. pi = 20
1. staff (default) = 30

*Note: the concept of collaborator and staff within the project context are merely assignments 
of PI and/or staff roles* 

In Role.php Add:

        use Illuminate\Database\Eloquent\Builder;

Add:

        public function permissions()
        { 
            // This bit filters the role options to only those levels below the user's own role.
            static::addGlobalScope('rank', function (Builder $builder) {
                $MyRole = \Auth::user()->role->rank;
                // \Log::info('Role: ' . $MyRole);
                $builder->where('rank', '>', $MyRole);
            });
            return $this->belongsToMany(Voyager::modelClass('Permission'));
        }

Browsing of other users is suppressed by creating an override file for voyager browse.blade.php In 
the copied file is a look up for obtaining the role/rank of the logged in user and comparing it with 
the role/rank of the line item in the user table displayed. This skips those with ranks that are 
lower (admin = 0 staff = 30)

## Configure BREAD and Models within Laravel/Voyager

Creat projects model and use as a target for creating a new BREAD for projects based on the projects 
table. The PI is defined by a relationship to the users table. __Watch the direction of the pivot.__

In the relationship options add a scope which sits in the user model. This is a sample of the logic.

        public function scopeActive($query)
        {
            // In the project details the owner of the porject can be as follows:
            // Admin = any PI - Logic update below
            // Owner = any PI - Logic update below
            // PI = self - Logic update below
            // Collab = read only - setting is in view custom_relationship_dropdown.blade.php
            // Staff = no access - controlled in bread setting for role
            
            // Save this bit for now just cause it works 
            // $users = \DB::table('users')
            // ->join('roles', 'users.role_id', '=', 'roles.id')
            // ->select('users.id')
            // ->where('roles.name', '=', 'pi')->get();
            // [{"id":2},{"id":3}] 
            // Log::info('TEST: ' . old($row->field));

            if( \Auth::user()->hasRole('admin')  || \Auth::user()->hasRole('owner') ){
                // Log::info('qualifies');
                $query->join('roles', 'users.role_id', '=', 'roles.id')
                ->select('users.name')
                ->where('roles.name', '=', 'pi');             
            }elseif( \Auth::user()->hasRole('pi')){
                // Is PI and can only assign to self
                $query->where('id', \Auth::user()->id);
            }
            return $query;
        } 

## Configure Custom Views

When defining the custom view in the BREAD form use the name of the template without the
path or the ".blade.php" portion of the file name. Template overrides are created by the existance 
of a template file with the same name/structure under app/vendor.

JSON Example:

        "view": "custom_select_dropdown"

Create a custom browse template to filter out admin from list of users when a non-admin is viewing. 

* Note: Other admin have to be created with:* 

        php artisan voyager:admin your@email.com

* Note: AJAX calls go through the VoyagerBaseController.php and return JSON without rendering a view. 
A custom controller is specified in some cases.

## Configure Policies

Policy files allow adding rules for access at the BREAD level. Sample:

        class ProjectPolicy extends BasePolicy { 
            public function read(User $user, $project) { 
                return $user->id === $project->user_id || $this->checkPermission($user, $project, 'read'); 
            } 
        }

## Dashboard

The dashboard is created via route >> controller >> view. The voyager route handles "/" and directs to app/Http/Controllers/DashboardController.php. The controller does all the SQL Calls and rendering of grids which are returned to the view. The progress bar is created with a combination of CSS in the makeGrid function and the CSS section of the view "voyager::index" which has an override file in the app section.

The cards are all managed in the CSS on the view file.

## Debugging

Tailing the log file and setting Log::info() is very useful.

        tail -f storage/logs/laravel.log

For DB queries, run \DB::enableQueryLog() before you run the query and then use dd(\DB::getQueryLog()) after to see what was actually run.

