<template>
    <li class="MainMenuMegaDropDownContent__element">
        <div class="MainMenuMegaDropDownContent__linkColumn">
            <div :class="['LinksColumn__toggle', {'is-expanded' : subDropdown}]" role="button" tabindex="0">
                <p v-if="!megaDropdown.meta.title_link" class="LinksColumn__title" @click="toggleSubDropdown">{{megaDropdown.title}}</p>
                <a v-if="megaDropdown.meta.title_link" :title="megaDropdown.title" class="LinksColumn__title" :href="megaDropdown.url">{{megaDropdown.title}}</a>
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 185.343 185.343" class="LinksColumn__svgArrowRight">
                    <path d="M51.707 185.343a10.692 10.692 0 0 1-7.593-3.149 10.724 10.724 0 0 1 0-15.175l74.352-74.347L44.114 18.32c-4.194-4.194-4.194-10.987 0-15.175 4.194-4.194 10.987-4.194 15.18 0l81.934 81.934c4.194 4.194 4.194 10.987 0 15.175l-81.934 81.939a10.678 10.678 0 0 1-7.587 3.15z"></path>
                </svg>
            </div>
            <ul :class="['LinksColumn__list', {'is-visible' : subDropdown}]">
                <li v-for="(megaItems, megaItemsIndex) in megaDropdown.children[0].children" :key="megaItemsIndex" class="LinksColumn__listElement">
                    <a :title="megaItems.title" class="LinksColumn__link" :href="megaItems.url">{{megaItems.title}}</a>
                </li>
            </ul>
            <ul v-if="megaDropdown.children[1]" :class="['LinksColumn__list', {'is-visible' : subDropdown}]">
                <li v-for="(megaLinks, megaLinksIndex) in megaDropdown.children[1].children" :key="megaLinksIndex" class="LinksColumn__listElement">
                    <a :href="megaLinks.url" :title="megaLinks.title" class="LinksColumn__link LinksColumn__ctaLink" target="_blank" rel="noopener">
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 185.343 185.343" class="LinksColumn__linkSvgArrowRight">
                            <path d="M51.707 185.343a10.692 10.692 0 0 1-7.593-3.149 10.724 10.724 0 0 1 0-15.175l74.352-74.347L44.114 18.32c-4.194-4.194-4.194-10.987 0-15.175 4.194-4.194 10.987-4.194 15.18 0l81.934 81.934c4.194 4.194 4.194 10.987 0 15.175l-81.934 81.939a10.678 10.678 0 0 1-7.587 3.15z"></path>
                        </svg>{{megaLinks.title}}
                    </a>
                </li>
            </ul>
        </div>
        <div v-if="megaDropdown.meta.hasImage" class="MainMenuMegaDropDownContent__imageColumn">
            <a :href="megaDropdown.meta.imageLink" :title="megaDropdown.meta.imageTitle" class="ImageColumn__title" target="_blank" rel="noopener">
            <p>{{megaDropdown.meta.imageTitle}}</p>
            <picture class="LazyPictureElement LazyPictureElement_loaded ImageColumn__image">
                
                <img :src="megaDropdown.meta.image" :alt="megaDropdown.meta.imageTitle" class="PictureElement__imgDefault">
                
            </picture>
            </a>
        </div>
    </li>
</template>

<script>
    export default {
        name: "SubDropdown",
        props: {
            megaDropdown: Object,
            megaDropdownIndex: Number
        },
        data(){
            return {
                subDropdown: false
            }
        },
        methods: {
            toggleSubDropdown(){
                this.subDropdown = !this.subDropdown
            }
        }
    }
</script>

<style scoped>

</style>
