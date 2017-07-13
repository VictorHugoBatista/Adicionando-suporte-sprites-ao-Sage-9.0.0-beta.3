# Adicionando suporte sprites ao Sage 9.0.0-beta.3

## Requisitos
* Ter o Yarn instalado na máquina.

## Passos
* Adicione o pacote _webpack-spritesmith_ como dependência de desenvolvimento pelo _Yarn_.
  * Comando __yarn add webpack-spritesmith --dev__.
* Importe o pacote ao arquivo webpack.config.js, adicionando a linha abaixo junto das outras importações.
  ```javascript
  const SpritesmithPlugin = require('webpack-spritesmith');
  ```
* Substitua o código abaixo pelo novo código, com o plugin SpriteSmith inicializado:
  ```javascript
  if (config.enabled.watcher) {
    webpackConfig.entry = require('./util/addHotMiddleware')(webpackConfig.entry);
    webpackConfig = merge(webpackConfig, require('./webpack.config.watch'));
  }
  ```
  para
  ```javascript
  if (config.enabled.watcher) {
    webpackConfig.entry = require('./util/addHotMiddleware')(webpackConfig.entry);
    webpackConfig = merge(webpackConfig, require('./webpack.config.watch'));
  } else {
    // Geração de sprites automáticos. Não executa no watch (yarn run start).
    const spriteSmith = new SpritesmithPlugin({
      src: {
        cwd: config.paths.assets + '\\images\\sprites',
        glob: '*.png'
      },
      target: {
        image: config.paths.assets + '\\images\\sprite.png',
        css: config.paths.assets + '\\styles\\common\\_sprites.scss'
      },
      apiOptions: {
        cssImageRef: '../images/sprite.png'
      }
    });
    webpackConfig.plugins.push(spriteSmith);
  }
  ```
 * Crie o diretório _sprites_ dentro de __raiz-sage/resources/assets/images__. Você irá adicionar as imagens à ser adicionadas ao plugin neste diretório.
 * Adicione os caminhos __resources/assets/images/sprite.png__ e **resources/assets/styles/common/_sprites.scss** ao arquivo _.gitignore_. Esses são os arquivos gerados automaticamente pelo _spritesmith_ e não precisam ser adicionados ao repositório.
  * Crie o arquivo __.stylelintignore__ na raíz do tema e adicione o caminho do arquivo __resources/assets/styles/common/_sprites.scss__. O arquivo ___sprites.scss__ é gerado automaticamente pelo _spritesmith_ e não segue oa padrões de lint do tema, logo, precisa ser ignorado para que o build seja permitido.

## Referências
* https://yarnpkg.com/pt-BR/docs/usage
* https://github.com/mixtur/webpack-spritesmith
* https://stylelint.io/user-guide/configuration/#stylelintignore

