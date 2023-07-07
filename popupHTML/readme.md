# popup.js

## Встроенные модули
|Название|Код|Назначение|
|:-|:-|:-|
|```jquery```|```<script src="https://code.jquery.com/jquery-3.6.1.min.js"></script>```|функции jq|
|```animate```|```<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"> ```|Подключение класснов для анимации|


## Шаблоны
### HTML
```html
<div class='<класс попапа>'>
    <div class='<класс попапа>__shell animate__animated'></div>
    <div class='<класс попапа>__body animate__animated'>
            
        <article class='<класс попапа>__body__container'>
            <div class="<класс попапа>__body__container__close">X</div>

                <!-- Внутренности попапа -->

        </article>
    </div>
</div>
```

### scss
```scss
.<класс попапа>{
    position: relative;
    z-index: 99;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: none;
    &__shell{
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: rgba(0, 0, 0, 0.5);
    }
    &__body{
        position: fixed;
        // кординаты и размеры попапа
        &__container{
            &__close{
                // Свойства закрывающей кнопки
            }  
        }
    }
}
```

### css
```css
.<класс попапа>{
    position: relative;
    z-index: 99;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: none;
}
.<класс попапа>__shell{
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
}
.<класс попапа>__body{
    position: fixed;
}
.<класс попапа>__body__container{

}
.<класс попапа>__body__container__close{

}
```

### Исходнный код popup.js
```js
const animateMenuForm = {
    In_shell: 'animate__fadeIn',
    Out_shell: 'animate__fadeOut',

    In_body: 'animate__fadeIn',
    Out_body: 'animate__fadeOut'
}

$(document).ready(()=>{

    function PopUp(popup){
        return {
            popup: popup,
            open: $('.' + `${popup.attr('class')}` + '__open'),
            close: $('.' + `${popup.attr('class')}` + '__body__container__close'),
            body: popup.find('.' + `${popup.attr('class')}__body`),
            shell: popup.find('.' + `${popup.attr('class')}__shell`),
        }
    }


    function open(popup, animate){
        popup.popup.css('display', 'block');
        popup.body.removeClass(animate.Out_body)
        popup.body.addClass(animate.In_body)
        // $('body').css('overflow', 'hidden');

        popup.shell.removeClass(animate.Out_shell)
        popup.shell.addClass(animate.In_shell)
    }

    function close(popup, animate){
        popup.body.removeClass(animate.In_body)
        popup.body.addClass(animate.Out_body)
        // $('body').css('overflow', 'auto');

        popup.shell.removeClass(animate.In_shell)
        popup.shell.addClass(animate.Out_shell)

        setTimeout(() => {
            popup.popup.css('display', 'none') 
        }, 500);
    }


    function PopStart(popup,animate) {

        PopUp(popup).open.click(() => {
        open(PopUp(popup),animate);
        })
        PopUp(popup).shell.click(()=>{
            close(PopUp(popup),animate);
        })
        PopUp(popup).close.click(()=>{
            close(PopUp(popup),animate);
        })
        // $(window).scroll(()=>{
        //     close(PopUp(popup),animate);
        // })
    }

    //Попапы, неменяющиеся 
    PopStart($('.<класс попапа>'), animateMenuForm);
})
```