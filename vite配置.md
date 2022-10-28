# vite打包配置

## 一、文件目录

build

-- vite.base.config.js

-- vite.dev.config.js

-- vite.prod.config.js

index.html

main.js

vite.config.js

和webpack最不同的是vite的入口不是js文件，而是我们的html并且看我们的html引入了上面js文件，所以目录结构也不像webpack需要将html放入public里面

## 二、基本结构

### vite.config.js

import viteBaseConfig from  'vite.base.config.js'

import viteDevConfig from 'vite.dev.config.js'

import viteProdConfig from 'vite.prod.config.js'

上面我们引入其他的配置文件

import { defineConfig } from 'vite' // 引入defineConfig方式配置可以更好的在vscode上获取提示 也是因为对ts的全面支持

let desipotionObj={	//这里我们配置这个对象去区分生成环境和开发环境的打包配置

​		'build':Object.assign({},viteBaseConfig,viteProdConfig)

​		'serve'Object.assign({},viteBaseConfig,viteDevConfig)

}

//这里我们使用传入一个函数，然后函数会接收一个对象，对象内包含command当前启动是build还是serve 还有一个是mode属性包含当前是development还是production

const viteConfig=defineConfig(({command,mode})=>{

​		return desipotionObj[command]

})

### vite.base.config.js

import  { defineConfig } from 'vite'

export default defineConfig({

})

### vite.dev.config.js

import  { defineConfig } from 'vite'

export default defineConfig({

})

### vite.prod.config.js

import  { defineConfig } from 'vite'

export default defineConfig({

})

## 三、配置环境变量

import { defineConfig,loadEnv } from 'vite'

export default defineConfig(({mode})=>{

​			loadEnv(mode,proccess.cwd()), //这里会根据你的mode去找到对应的环境变量文件

​			envPrefix:'TEXT_'  这个可以改变默认的环境变量开头

})

### .env

VITE_ZBC=666

### .env.production

VITE_LQY=520

### .env.development

VITE_ZZZ=1314

我们的环境变量默认都要使用VITE_开头

如果要在我们的js或者vue中访问环境变量我们可以使用import.mate.env

## 四、css与less、sass的配置

import { defineConfig } from 'vite'

export default defineConfig({

​		css:{

​			modules:{

​				//localsConvention可以配置css模块化返回的对象是驼峰还是下划线如果有带only就是驼峰，如下的是下划线

​				localsConvention:"camelCase" ，

​				//scopeBehaviour可以配置当前模块化属于全局还是自身的local属于自身global属于全局

​				scopeBehaviour:"local",

​				//hashPrefix可以让其转换的hash值随机添加我们后面设置的字符串达到更高的不同

​				hashPrefix:"hello"，

​				//globalModulePaths可以配置我们不想包含模块化的css文件

​				globalModulePaths:[ "css路径" ]

​			},

​			//我们可以在preprocessorOptions中配置我们的less和sass等预编译语言

​			preprocessorOptions:{

​				less:{

​						//math的always可以让我们的less中写的数值可以运算

​						math:"always",

​						//globalVars可以让我们设置全局的变量共所有的less访问

​						globalVars:{

​								color:"white"

}

​						//additionalData中配置是我们全局使用的less路径

​						additionalData:less路径

}

//sass的配置和上面less的类似

}

}

})

## 五、postcss的配置

在vite内是完全兼容postcss的，我们只需要安装一个postcss-preset-env来配置一些我们的postcss预处理，确认我们postcss默认都要干那些事情

import  { defineConfig }  from  'vite'	

//首先导入我们安装的postcss-preset-env 默认需要postcss处理的配置预选项

import  postcssPresetEnv from 'postcss-preset-env'

export default=defineConfig({

​	//然后配置我们的postcss 里面的plugins数组将我们的导入的postcssPresetEnv放入即可	

​		postcss:{

​				plugins:[ 

​					postcssPresetEnv ()

]

}

})

### postcss.config.js

我们也可以去使用单独的postcss.config.js,vite会自动的去寻找个根目录下的这个文件并读取里面对于postcss的配置。

const postcssPresetEnv=require('postcss-preset-env');

module.exports={

​		plugins:[

​			postcssPresetEnv()

]

}

## 六、resolve相对路径的配置

import { defineConfig } from 'vite'

//我们先导入commonjs的path模块的resolve方法

import { resolve } from 'path'

export default=defineConfig({

//然后我们配置resolve的alias对象属性在里面指定我们特定的相对路径

​		resolve:{

​			alias:{

​				"@":resolve(__dirname,"../")

}

}

})

## 七、生产环境build的配置

import { defineConfig } from 'vite'

export default=defineConfig({

​		build:{

//因为我们vite在打包生产环境是依赖我们的rollup的，因为开箱即用所以vite自己配置了一套默认的配置，如果我们要修改就要在rollupOptions对象属性去配置一些特有的属性

​			rollupOptions:{

​				output:{

​						assetFileName:"[hash].[name].[ext]"

}

},

//assetInlineLimit可以帮我们去分辨如果小于后面给的值的静态文件帮我们转换为base64

​			assetInlineLimit:xxx,

//outDir可以让我们去配置打包好的文件存放的文件名

​			outDir:xxx，

//assetsDir可以让我们去配置js存放目录的名称

​			assetsDir:""

}

})

## 八、svg的配置和导入资源的路径配置

vite原生的支持我们的svg无需去做其他的配置。

### 路径参数

我们去导入我们的文件的时候可以在后面添加参数表示其导入格式

?url 表示按相对的路径给其

?raw 我们所有的文件在最原始的时候都是二进制流也就是buffer，这里就是将导入文件原生的二进制交出来

?worker 这里表示导入的文件是一个worker脚本

## 九、plugins插件的配置

import { defineConfig } from 'vite'

//导入我们所需的插件

import { xxx } from 'xxx'

export default=defineConfig({

​		plugins:[

​			//里面放置我们需要的插件，插件都是义函数的形式去调用

​			如果我们需要指定插件的执行顺序我们需要使用一个大括号将插件调用包起来,并配置一个enforce属性去配置顺序

]

})

此外我们还可以自定义插件 ， vite给我们提供了许多的生命周期钩子让我们去设计我们的插件，详情可见官网

