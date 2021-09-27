<template>
    <div class="ContentZone ContentZone__footer">
        <div class="Prefooter">
            <div v-if="back_to_top" class="Prefooter__buttonContainer">
                <button type="button" class="Prefooter__returnToTop" @click="scrollToTop()">
                    <div class="Button__content">{{back_to_top}}</div>
                </button>
            </div>
            <ul v-if="prefooter && prefooter.active == 1" class="Prefooter__content Slice Slice_childrenMobileWrap">
                <li v-for="(prefooterItem, prefooterItemIndex) in prefooter.schema" :key="prefooterItemIndex" class="SubPrefooter Slice__element">
                    <span class="SubPrefooter__link">
                        <picture class="LazyPictureElement LazyPictureElement_loaded SubPrefooter__icon is-contained">
                            <img :src="prefooterItem.icon" alt="SubPrefooter Icon" class="PictureElement__imgDefault">
                            <noscript>
                                <img :src="prefooterItem.icon" alt="SubPrefooter Icon" />
                            </noscript>
                        </picture>
                        <p class="SubPrefooter__text">
                            <span class="SubPrefooter__mainText">{{prefooterItem.title}}</span>
                            <span class="SubPrefooter__SecondaryText">{{prefooterItem.meta.text}}</span>
                        </p>
                    </span>
                </li>
            </ul>
    </div>
    <footer itemscope="" itemtype="http://schema.org/WPFooter" class="Footer">
        <div class="Footer__section">
            <div class="SeoFooter"></div>
        </div>
        <div class="Footer__section">
            <div v-if="mainNavigation" class="Footer__columns">
                <div v-for="(mainNav, mainNavIndex) in mainNavigation" :key="mainNavIndex" class="FooterColumn Footer__column" role="button" tabindex="0">
                    <p v-if="mainNav.active" class="FooterColumn__titleColumn" @click.prevent="toggle(mainNavIndex)">
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 30 30" class="FooterColumn__SvgIcon">
                            <path v-if="!isVisible(mainNavIndex)" d="M1.822 7.527a1.058 1.058 0 0 0-1.46 1.532l14.384 13.707L29.624 9.071a1.059 1.059 0 0 0-1.432-1.558l-13.42 12.354-12.95-12.34z"></path>
                            <path v-if="isVisible(mainNavIndex)" d="M28.177 22.473a1.058 1.058 0 0 0 1.46-1.532L15.253 7.234.377 20.93a1.058 1.058 0 1 0 1.433 1.557l13.419-12.353 12.948 12.339z"></path>
                        </svg>{{mainNav.title}}
                    </p>
                    <ul v-if="mainNav.active" :class="['FooterColumn__linksList', {'is-visible' : isVisible(mainNavIndex)}]">
                        <li v-for="(mainNavItem, mainNavItemIndex) in mainNav.children" :key="mainNavItemIndex" class="FooterColumn__linkContainer">
                            <a v-if="mainNavItem.active" :title="mainNavItem.title" :target="mainNavItem.target" class="FooterColumn__link" :href="mainNavItem.url">{{mainNavItem.title}}</a>
                        </li>
                    </ul>
                </div>
            </div>
            <div v-if="socialLinks.schema[0]" class="FooterSocialNetwork Footer__social">
                <p class="FooterSocialNetwork__titleColumn">{{socialLinks.schema[0].title}}</p>
                    <ul class="FooterSocialNetwork__socialNetworkLinks">
                        <li v-for="(social, socialIndex) in socialLinks.schema[0].children" :key="socialIndex" class="FooterSocialNetwork__socialNetworkElement">
                            <a :href="social.url" :title="social.url" class="FooterSocialNetwork__socialNetworkLink" target="_blank" rel="noopener">
                                <picture class="LazyPictureElement LazyPictureElement_loaded FooterSocialNetwork__socialNetworkIcon">
                                    <img :src="social.icon" alt="social-logo" class="PictureElement__imgDefault">
                                    <noscript>
                                        <img :src="social.icon" alt="social-logo" />
                                    </noscript>
                                </picture>
                            </a>
                        </li>
                    </ul>
                </div>
            </div>
            <nav v-if="legalNavigation" class="FooterLegal">
                <ul class="FooterLegal__list">
                    <li v-for="(legal, legalIndex) in legalNavigation" :key="legalIndex" class="FooterLegal__element">
                        <a :title="legal.title" class="FooterLegal__link" :href="legal.url">{{legal.title}}</a>
                    </li>
                </ul>
                <span v-if="basicInformation.meta.footer_trademark" class="FooterLegal__copyright">{{footer_trademark}}</span>
            </nav>
        </footer>
    </div>
</template>

<script>
    import axios from "axios";

    export default {
        name: "Footer",

        props: {
            url: {
                type: String
            }
        },

        created() {
            if ("HEADER_FOOTER_SETTINGS" in window) {
                this.fetchNavigation(window.HEADER_FOOTER_SETTINGS.apiUri);
            } else {
                if (this.url) {
                    this.fetchNavigation(this.url);
                }
            }
        },

        data() {
            return {
                visibleIndex: null,
                basicInformation: [],
                mainNavigation: [],
                prefooter: [],
                socialLinks: [],
                legalNavigation: []
            }
        },

        computed: {
            footer_trademark() {
                return this.basicInformation && this.basicInformation.meta && this.basicInformation.meta.footer_trademark;
            },
            back_to_top() {
                return this.basicInformation && this.basicInformation.meta && this.basicInformation.meta.back_to_top;
            }
        },

        methods: {
            fetchNavigation(apiUri) {
                axios.get(apiUri)
                    .then((response) => {
                        this.basicInformation = response.data;
                        this.prefooter = response.data.prefooter;
                        this.mainNavigation = response.data.footer.schema;
                        this.socialLinks = response.data.footer_social;
                        this.legalNavigation = response.data.footer_legal.schema;
                    })
            },

            isVisible(itemIndex) {
                return this.visibleIndex === itemIndex;
            },

            toggle(itemIndex) {
                if (this.visibleIndex === itemIndex) {
                    this.visibleIndex = null;
                } else {
                    this.visibleIndex = Number(itemIndex);
                }
            },

            scrollToTop() {
                window.scrollTo({ top: 0, behavior: 'smooth' });
            }
        }
    }
</script>

<style scoped>
    @import './assets/css/renault_min.css';

    @font-face{
        font-family:"readBeta2 sans-serif";
        font-weight:400;
        font-display:swap;
        src:url(./assets/fonts/readBeta2-Light.woff2) format("woff2")
    }
    @font-face{
        font-family:"readBeta2 sans-serif";
        font-weight:700;
        font-display:swap;
        src:url(./assets/fonts/readBeta2-Medium.woff2) format("woff2")
    }
    @font-face{
        font-family:"RenaultLife";
        font-weight:300;
        font-display:swap;
        src:url(./assets/fonts/RenaultLifeWeb-Light.woff) format("woff")
    }
    @font-face{
        font-family:"RenaultLife";
        font-weight:400;
        font-display:swap;
        src:url(./assets/fonts/RenaultLifeWeb-Regular.woff) format("woff")
    }
    @font-face{
        font-family:"RenaultLife";
        font-weight:700;
        font-display:swap;
        src:url(./assets/fonts/RenaultLifeWeb-Bold.woff) format("woff")
    }
    @font-face{
        font-family:NouvelR;
        font-weight:400;
        font-display:swap;
        src:url(./assets/fonts/NouvelR-Regular.woff2) format("woff2")
    }
    @font-face{
        font-family:NouvelR;
        font-weight:700;
        font-display:swap;
        src:url(./assets/fonts/NouvelR-Bold.woff2) format("woff2")
    }
    @font-face{
        font-family:Pictos;
        font-style: normal;
        font-weight: 400;
        src:url(./assets/fonts/PictosComplete-Regular.woff) format("woff")
    }
</style>