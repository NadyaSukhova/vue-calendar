<template>
    <div class="header-content">
        <div class="header-text">{{ lang == "Rus" ? "Тестовое задание для" : "Test assignment for" }}
            <a href='https://spb.hh.ru/vacancy/127207729?hhtmFrom=chat' target='_blank'>
                {{ lang == "Rus" ? "вакансии JavaScript разработчика" : " JavaScript developer vacancy" }}
            </a>
        </div>
        <button @click='toggleLang'>{{ lang == "Rus" ? "Translate to Eng" : "Перевести на Рус" }}</button>
    </div>
</template>

<script>
import '../assets/styles/header.css';

export default {
    name: 'Header',
    props: {
        lang: {
            type: String,
            default: 'Rus'
        },
        toggleLang: {
            type: Function,
            required: true
        }
    }
}
</script>