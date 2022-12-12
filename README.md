# laravel-vite-vue-inertia-app

# Configurando um projeto laravel
curl -s "https://laravel.build/nome-do-projetol?with=mysql" | bash 

## Se for utilizar o sail, subir o projeto e utilizar o comando sail antes dos comandos indicados
sail up -d

# Configuração da parte do "Servidor"

## Efetuar a instalação do Inertia
composer require inertiajs/inertia-laravel

## Configurar o template padrão (app.blade.php)
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0" />
    @vite(['resources/css/app.css','resources/js/app.js'])
    @inertiaHead
  </head>
  <body>
    @inertia
  </body>
</html>
```
## Configurar o middlware do inertia e adicionar o Handler em App\Http\Kernel
php artisan inertia:middleware

'web' => [
    // ...
    \App\Http\Middleware\HandleInertiaRequests::class,
],

# Configuração da parte do "Cliente"

## Instalar as dependencias com node
npm install @inertiajs/inertia @inertiajs/inertia-vue3 vue@next @vitejs/plugin-vue

## Inicializar o app no arquivo Javascript de configuração (boot.js ou app.js)
```
import "./bootstrap";

import { createApp, h } from "vue";
import { createInertiaApp } from "@inertiajs/inertia-vue3";
import { resolvePageComponent } from "laravel-vite-plugin/inertia-helpers";

createInertiaApp({
    title: (title) => `${title} - ${appName}`,
    resolve: (name) =>
        resolvePageComponent(
            `./Pages/${name}.vue`,
            import.meta.glob("./Pages/**/*.vue")
        ),
    setup({ el, app, props, plugin }) {
        return createApp({ render: () => h(app, props) })
            .use(plugin)
            .mount(el);
    },
});
```

## Configurar o vue no arquivo vite.config.js
```
import { defineConfig } from 'vite';
import laravel from 'laravel-vite-plugin';
import vue from '@vitejs/plugin-vue';

export default defineConfig({
    plugins: [
        laravel({
            input: ['resources/css/app.css', 'resources/js/app.js'],
            refresh: true,
        }),
        vue({
            template: {
                transformAssetUrls: {
                    base: null,
                    includeAbsolute: false
                }
            }
        }),
    ],
});
```
