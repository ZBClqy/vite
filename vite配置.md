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