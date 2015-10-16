stream-switch
=========

>A stream condition for switch/case

>[![Dependencies][david-image]][david-url]

[david-image]: http://img.shields.io/david/nuintun/gulp-cmd.svg?style=flat-square
[david-url]: https://david-dm.org/nuintun/gulp-cmd

###Usage
```js
var path = require('path');
var join = path.join;
var relative = path.relative;
var gulp = require('gulp');
var cmd = require('gulp-cmd');
var alias = {
  'import-style': 'util/import-style/1.0.0/import-style'
};

// Fixed css resource path
function onpath(path, property, file, wwwroot){
  if (/^[^./\\]/.test(path)) {
    path = './' + path;
  }

  if (path.indexOf('.') === 0) {
    path = join(dirname(file), path);
    path = relative(wwwroot, path);
    path = '/' + path;
    path = path.replace(/\\+/g, '/');
  }

  path = path.replace('assets/', 'online/');

  return path;
}

// Task
gulp.task('default', function (){
  gulp.src('assets/js/**/*.js', { base: 'assets/js' })
    .pipe(cmd({
      alias: alias,
      ignore: ['jquery'],
      include: function (id){
        return id.indexOf('view') === 0 ? 'all' : 'self';
      },
      css: { onpath: onpath }
    }))
    .pipe(gulp.dest('online/js'));
});
```

###API
####cmd(options)
#####  *options*
- map ```Array```
  
  ����ģ��ID·��ӳ���޸ģ�������·��ת����
  
- vars ```Object```
  
  ģ��·��������ʱ����ȷ������ʱ����ʹ�� vars ���������á�

- paths ```Object```
  
  ��Ŀ¼�Ƚ������Ҫ��Ŀ¼����ģ��ʱ������ʹ�� paths ������д��

- alias ```Object```
  
  ��ģ���ʶ�ܳ�ʱ������ʹ�� alias ���򻯡�
>ע�⣺*[import-style](https://github.com/nuintun/import-style) Ϊ������ʽ����ģ�飬�������� alias �Ա���ȷ��ת����ģ�飬��ģ����Ҫ�Լ����ز�������ӦĿ¼�� vars paths alias �ɲο� [seajs](https://github.com/seajs/seajs/issues/262) ������*

- cache ```Boolean```
  
  �ļ��ڴ滺�棬ת����ɵ��ļ�����ʱ�洢���ڴ����Ա�����ת��Ч�ʡ�

- wwwroot ```String```
  
  ��վ��Ŀ¼���ã�·������� ```gulpfile.js``` Ŀ¼��

- idleading ```String|Function```
  
  ģ�� id ת��ģ�壬Ĭ�� ```{{name}}/{{version}}/{{file}}```�� ���������� vinyl �ļ��� relative ����ת������������ gulp.src �� [base](https://github.com/wearefractal/vinyl) �����������ã�base ��ͬ�� seajs �� [base](https://github.com/seajs/seajs/issues/262) ���á�

- rename ```Object|Function```
  
  �������ļ����� ```debug``` �� ```min``` �������ÿ�ѡ���򿪺��ļ������Զ���� -debug �� -min ��׺��debug ��ʱ min ������Ч���� rename �Ǻ�����ʱ��Ҫ���� ```{ prefix: '', suffix: '' }``` ��ʽ�Ķ��󣬷ֱ��Ӧǰ׺�ͺ�׺��

- plugins ```Object```
  
  �ļ�ת����������Ը�дĬ�ϲ����Ҳ�ɶ����²����ƥ��� ```vinyl``` �ļ��ᾭ�������ת��������������ֱ���Ϊ������ ```.``` �ļ���չ����

- include ```String```
  
  ģ���װģʽ��Ĭ�� ```relative```����ѡ ```all``` �� ```self```���ֱ��Ӧ����1���ϲ���������ļ�����2���ϲ����������ļ�����3�����ϲ��κ��ļ���

- css ```Object```
  
  ת�� css �� js �����ã��� ```onpath``` �� ```prefix``` �������ÿ�ѡ����������Ϊ ```String|Function```����Ӧ css �ļ�����Դ�ļ�·�����������ǰ׺��

- ignore ```Array```
  
  ģ��ϲ���Ҫ���Ե�����ģ�飬֧��·���� vars paths alias ���ã���֧�����·����Ĭ�Ϻ��ԣ����� ```/``` ��ͷ��·������ wwwroot Ѱ�ң� �������� base Ѱ�ҡ�

>ע�����*ģ�� id �� ```/``` ��β��Ĭ���� ```index.js``` ���� ```index.css``` ��ȫ*�� id �� ```/``` ��ͷ��ģ���� wwwroot ·��Ѱ�ҡ�id ����򻯣����� id �����Զ���ȫ js ��׺���� seajs ��[ģ���ʶ](https://github.com/seajs/seajs/issues/258)�е� *�ļ���׺���Զ���ӹ���* �е����𣬵������ƻ� seajs �Ĺ���ֻ����Ӧģ�鲻��������ͺϲ���css �� import �����ԭ��һ�£���Ҫע����Ǿ�����Ҫ����Զ����Դ��
