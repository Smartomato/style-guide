#Smartomato Style Guide

Привет! Мы разрабатываем методологию написания стилей, которая будет удовлетворять следущим как требованиям уже существующих методологий, так и нашим собственным представлениям о прекрасном:

* методология должна иметь низкий порог вхождения
* подход должен быть гибким, а отказ от него приводить к значительному ухудшению структуры кода
* должны учитываться современные тендеции в использовании препроцессоров и компонентов

При разработке мы вдохновлялись концепциями OOCSS, SMACSS, и BEM. Примером нашего решения служит данный репозиторий.

Будем использовать следущие термины:
* Base
* Tool
* Layout
* Module
* Component

Base — включает в себя сброс стандартных стилей браузера и набор базовых стилей для HTML тегов. В нем не используются селекторы классов и идентификаторов.
```css
body, form {
    margin: 0;
    padding: 0;
}

a {
    color: #039;
}

a:hover {
    color: #03F;
}
```

Tools — набор переменных и кастомных миксинов. Здесь не описываются никакие стили, а названия файлов начинаются с символа underscore, например «_settings.styl».

```sass
$statusNew       = #35ADE8
$statusConfirmed = #A855A9
$statusPreparing = #E2544B
$statusPrepared  = #F59500
$statusDelivery  = #FBC400
$statusCompleted = #2ECC71
$statusCanceled  = #989898

smallscreen($break = 1199px)
  @media only screen and (max-width: $break)
    {block}

midscreen($break = 1200px, $max = 1400px)
  @media only screen and (min-width: $break) and (max-width: $max)
    {block}

bigscreen($break = 1400px)
  @media only screen and (min-width: $break)
    {block}

```

Layout — содержит стили, которые относятся только к макету сайта. Они определяют расположение основных контентных блоков на странице. В большенстве случаев достаточно одного файла, в котором будет содержаться описание различных лэйатуов приложения. В отличае от SMACSS мы не будем использовать префиксы перед классами для лэйаута, нам вполне хватит использования семантичных названий, некоторые из которых заимствованы из названий html5 тегов.

```css
.header, .article, .footer {
    width: 960px;
    margin: auto;
}

.article {
    border: solid #CCC;
    border-width: 1px 0 0;
}

```

Component — введение данного набора сущностей навеяно последними тенденциями по выделению интеракивных элементов страницы в отдельные компоненты (в частности веб-компоненты). В текущих проектах мы используем Ember-компоненты, однако данный подход легко переносится и на другие фрэймворки. В контексте Ember-приложения возникла необходимость разделять универсальные компоненты, которые могут и скорее всего будут использоваться повторно (например заверстанные  в фирменном стиле селект-боксы) и "одноразовые" компоненты, которые выносятся для однообразности, но скорее всего не будут использованы повторно(модальное окно имеющее смысл только в контексте конкретного роута). Первый тип компонентов мы назовем Generic, второй — Specific. Для Generic компонентов хорошей практикой является использование префиксов, в Смартомато мы используем "sm-", это позволяет создать ui-kit который легко может быть перенесен и использован в другом проекте.

#### generic/sm-checkbox.styl
```sass
@import 'nib'
@import '../../tools/_colors'

.sm-checkbox
  user-select none
  display inline-block
  padding-left 30px
  cursor pointer

  > input[type=checkbox]
    display none

  > .label
    display inline-block
    position relative
    font-size 14px
    line-height 20px
    margin-top 15px
    color $textDarkGray

    &:before
      content ''
      display block
      position absolute
      box-sizing border-box
      width 20px
      height 20px
      left -30px
      top -1px
      background white
      border-radius 8px
      border 1px solid $lgrey

    &:after
      content ''
      display none
      position absolute
      box-sizing border-box
      width 6px
      height 10px
      border-radius 1px
      left -23px
      top 3px
      border-right 2px solid white
      border-bottom 2px solid white
      transform rotate(45deg)

  input[type=checkbox]:checked + .label
    color $textBlack

    &:before
      background $identity

    &:after
      display block


```
Все состояния чекбокса также определяются в этом файле. Код Generic компонента не нужно приводить, так как он может быть совершенно любым, главное изолировать его по классу корневого элемента.

Modules — самая неоднозначная сущность. Модули похожи на блоки в БЭМ, они определяют расположение контентных блоков внутри лэйаута. Модуль может быть вложен в другой модуль. Работа с этой частью верстки допускает отказ от принципов DRY, код описывающий модули может не использоваться повторно и чаще всего принадлежит контексту конкретного роута.
Модули также реализуют изоляцию стилей по классу корневого элемента. В остальных случаях стили приложения не должны быть изолированы.


![Screenshot1](https://raw.githubusercontent.com/Smartomato/style-guide/master/app/assets/images/scr1.png)
Модальное окно — хороший пример модуля
![Screenshot2](https://raw.githubusercontent.com/Smartomato/style-guide/master/app/assets/images/scr2.png)


#### modules/curier.styl
```sass
.courier-item
  min-height 75px
  display block
  color #757575
  padding 10px
  border 1px solid transparent
  border-top 1px solid #e4e4e4

  .avatar
    border-radius 1000px
    width 50px
    height 50px
    float left
    margin-top 5px
    background-color #fbfbfb

  .name,
  .organization,
  .phone
    margin-left 70px

  .name
    font-weight 400

  .organization
    font-size 12px


  .avatar-wrapper
    float left
    width 118px
    height 118px
    border-radius 118px
    margin-left 20px
    overflow hidden

    img.avatar
      width 100%
      height 100%
      border-radius 118px

  .contact-info
    float left
    margin-left 30px
    width 390px
    box-sizing initial

  .credentials
    margin-top 20px
    float none
    clear both
    width 580px
    padding-left 20px
    clearfix()

    h4
      margin-top 20px

    .sm-textfield
      float left
      width 260px
      margin-right 20px
  .sm-textarea
    margin-left 20px
    margin-top 20px
    width 540px
    float none

    textarea
      height 100px
      resize vertical


```
