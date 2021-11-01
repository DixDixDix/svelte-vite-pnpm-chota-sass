1. Использую pnpm чтобы не увеличивать папку node_modules
2. Использую сборщик vite.js для скорости
3. В vite.js использую прокси на http://ko73 чтобы избежать CORS при авторизации.

1. Создаю проект vite:
npm init @vitejs/app
2. Выбираю svelte
3. Захожу в созданную папку
4. Устанавливаю зависимости: pnpm i
5. Прокси настраиваю в файле vite.config.js

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [svelte()],
  server: {
    proxy: {
      '/api': {
        target: 'http://ko73',
        changeOrigin: true,
	    rewrite: (path) => path.replace(/^\/api/, '')
 	    }
    }
  }
})