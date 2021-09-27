<template>

    <div class="ContentZone ContentZone__menu">
        <header :class="['MainHeader', {'is-menuOpened' : isMobileOpened}]">
            <h1 class="u-hidden-wording">{{welcomeText}}</h1>
            <div class="MainHeader__mobile">
                <a title="Renault logo" class="MainHeader__logoLink" data-track="click" data-track-button-text="logo-renault-header" data-track-destination="/" data-track-location-in-page="primary-nav" data-track-event="navigationClick" :href="basicInformation.site_url">
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 66 86" class="MainHeader__logo">
                        <path d="M52.3 43l-23 43H23L0 43 22.9 0h6.5L6.5 43l19.6 36.9L45.7 43 34.3 21.5l3.3-6.1L52.3 43zM42.5 0h-6.6L13.1 43l14.7 27.6 3.2-6.1L19.6 43 39.2 6l19.6 37-22.9 43h6.6l22.8-43L42.5 0z"></path>
                    </svg>
                </a>
                <button @click="toggleMobile" class="MainHeader__icon" title="Izbornik">
                    <svg viewBox="0 0 30 30" class="SvgIcon MainHeader__burger">
                        <path d="M28.75 2.5H1.25a1.25 1.25 0 0 0 0 2.5h27.5a1.25 1.25 0 0 0 0-2.5zM1.25 16.25h17.5a1.25 1.25 0 0 0 0-2.5H1.25a1.25 1.25 0 0 0 0 2.5zM23.75 25H1.25a1.25 1.25 0 0 0 0 2.5h22.5a1.25 1.25 0 0 0 0-2.5z">
                        </path>
                    </svg>
                </button>
            </div>
            <div class="MainHeader__desktop">
                <button @click="toggleMobile" class="MainHeader__overlay" tabindex="0">
                    <svg viewBox="0 0 30 30" class="MainHeader__close">
                        <path d="M29.999 2.512L27.5.012 14.999 12.504 2.498.012l-2.497 2.5L12.5 15.001.001 27.488l2.497 2.5 12.501-12.49L27.5 29.988l2.497-2.5-12.499-12.487z"></path>
                    </svg>
                    <span class="u-hidden-wording">close</span>
                </button>
                <div class="MainHeader__content">
                    <div class="MainHeader__top">
                        <div v-if="topNavigation" :class="['HeaderSites', {'is-visible' : topNavMenuMobileActive}]">
                            <a @click.prevent="mobileTopMenuVisible(topNavIndex)" v-for="(topNav, topNavIndex) in topNavigation" :key="topNavIndex" :title="topNav.title" class="HeaderSites__site" :href="topNav.url">{{topNav.title}}</a>
                        </div>
                    </div>
                    <div class="MainHeader__bottom">
                        <a title="Renault logo" class="MainHeader__logoLink" data-track="click" data-track-button-text="logo-renault-header" data-track-destination="/" data-track-location-in-page="primary-nav" data-track-event="navigationClick" :href="basicInformation.site_url">
                            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 66 86" class="MainHeader__logo">
                                <path d="M52.3 43l-23 43H23L0 43 22.9 0h6.5L6.5 43l19.6 36.9L45.7 43 34.3 21.5l3.3-6.1L52.3 43zM42.5 0h-6.6L13.1 43l14.7 27.6 3.2-6.1L19.6 43 39.2 6l19.6 37-22.9 43h6.6l22.8-43L42.5 0z"></path>
                            </svg>
                        </a>
                        <div :class="['MainMenu', {'is-visible' : !topNavMenuMobileActive}]">
                            <ul v-if="mainNavigation" class="MainMenu__navList">
                                <li class="MainMenuEntry MainMenu__homeEntry">
                                    <a title="" class="MainMenuEntry__link" data-track="click" data-track-button-text="logo" data-track-destination="/" data-track-location-in-page="primary-nav" data-track-event="navigationClick" :href="basicInformation.site_url"></a>
                                </li>
                                <li v-for="(main, mainIndex) in mainNavigation" :key="mainIndex" :class="['MainMenuEntry', {'is-expanded' : isActiveDropdown(mainIndex)}]">
                                    <button :class="['MainMenuEntry__link', {'is-opened' : isActiveDropdown(mainIndex)}]" @click="toggleDropdown(mainIndex)">
                                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 185.343 185.343" class="SvgIcon MainMenuEntry__dropicon">
                                            <path d="M51.707 185.343a10.692 10.692 0 0 1-7.593-3.149 10.724 10.724 0 0 1 0-15.175l74.352-74.347L44.114 18.32c-4.194-4.194-4.194-10.987 0-15.175 4.194-4.194 10.987-4.194 15.18 0l81.934 81.934c4.194 4.194 4.194 10.987 0 15.175l-81.934 81.939a10.678 10.678 0 0 1-7.587 3.15z"></path>
                                        </svg>{{main.title}}
                                    </button>
                                    <div v-if="main.meta.vehicle_menu" :class="['EditorialContentZone', 'MainMenuEntry__megaDropdown', {'MainMenuEntry__megaDropdown_isHidden' : !isActiveDropdown(mainIndex)}]">
                                        <div class="MainMenuRangePicker MainMenuRangePicker_vertical">
                                            <button :style="vehicleNavIndex == 0 ? 'order:' + vehicleNavIndex : 'order:' + (vehicleNavIndex*2)" @click="toggleVehicleGroup(vehicleNavIndex)" v-for="(vehicleNav, vehicleNavIndex) in main.children[0].children" :key="vehicleNavIndex" :class="['MainMenuRangePicker__tab', 'MainMenuRangePicker__tab_pos0', {'is-lastActive' : isActive(vehicleNavIndex)}, {'is-active' : isActive(vehicleNavIndex)}]" data-track="click" data-track-event="toggle" data-track-toggle-type="tab-selector" :data-track-toggle-value="vehicleNav.title">
                                                <p class="MainMenuRangePicker__tabTitle MainMenuMegaDropDownContent__tab">{{vehicleNav.title}}</p>
                                            </button>
                                            <div :style="vehiclesIndex == 0 ? 'order:' + vehiclesIndex : 'order:' + (vehiclesIndex*2)" v-for="(vehicles, vehiclesIndex) in main.children[0].children" :key="100 + vehiclesIndex" :class="['MainMenuRangePicker__content', 'MainMenuRangePicker__content_pos0', {'is-lastActive' : isActive(vehiclesIndex)}, {'is-active' : isActive(vehiclesIndex)}]" id="tab-0">
                                                <div class="MainMenuRangeModels">
                                                    <div v-for="(vehicleGroupe, vehicleGroupeIndex) in vehicles.children" :key="vehicleGroupeIndex"  :class="[{'MainMenuModel' : !vehicleGroupe.meta.more_vehicles}, {'MainMenuRangeModels__element' : vehicleGroupe.meta.more_vehicles}]">
                                                        <figure v-if="!vehicleGroupe.meta.more_vehicles" class="MainMenuModel__imageWrapper">
                                                            <picture class="LazyPictureElement LazyPictureElement_loaded WebrenderPictureElement MainMenuModel__image is-ratio-forced">
                                                                <img :src="vehicleGroupe.icon" :alt="vehicleGroupe.title" class="PictureElement__imgDefault">
                                                            </picture>
                                                        </figure>
                                                        <a v-if="!vehicleGroupe.meta.more_vehicles" :title="vehicleGroupe.title" class="MainMenuModel__name" :href="vehicleGroupe.url">{{vehicleGroupe.title}}</a>
                                                        <div v-if="vehicleGroupe.meta.more_vehicles" class="MainMenuRangeModels__elementLinkInner">
                                                            <button class="ButtonMore MainMenuRangeModels__link" @click="openMore(vehicleGroupe.url)">
                                                                <span class="ButtonMore__picto">
                                                                    <span class="u-hidden-wording">more</span>
                                                                </span>
                                                            </button>
                                                            <a title="" class="MainMenuRangeModels__linkLabel" :href="vehicleGroupe.url">{{vehicleGroupe.title}}</a>
                                                        </div>
                                                    </div>
                                                </div>
                                            </div>
                                            
                                            <ul :style="'order:' + (main.children[0].children.length * 2 + 1)" class="MainMenuRangePicker__links">
                                                <li v-for="(vehicleLinks, vehicleLinksIndex) in main.children[1].children" :key="vehicleLinksIndex" class="MainMenuRangePicker__listElement">
                                                    <a :title="vehicleLinks.title" class="MainMenuRangePicker__link MainMenuRangePicker__ctaLink" target="_blank" :href="vehicleLinks.url">
                                                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 185.343 185.343" class="MainMenuRangePicker__svgArrowRight">
                                                            <path d="M51.707 185.343a10.692 10.692 0 0 1-7.593-3.149 10.724 10.724 0 0 1 0-15.175l74.352-74.347L44.114 18.32c-4.194-4.194-4.194-10.987 0-15.175 4.194-4.194 10.987-4.194 15.18 0l81.934 81.934c4.194 4.194 4.194 10.987 0 15.175l-81.934 81.939a10.678 10.678 0 0 1-7.587 3.15z"></path>
                                                        </svg>{{vehicleLinks.title}}
                                                    </a>
                                                </li>
                                            </ul>
                                        </div>
                                    </div>
                                    <div v-if="main.meta.dropdown" class="MainMenuEntry__dropdown">
                                        <ul class="MainMenuDropDownList">
                                            <li v-for="(dropdown, dropdownIndex) in main.children" :key="dropdownIndex" class="MainMenuDropDownList__element">
                                                <a :title="dropdown.title" class="MainMenuDropDownList__link" data-track="click" :data-track-button-text="dropdown.title" :data-track-destination="dropdown.url" data-track-location-in-page="secondary-nav" data-track-event="navigationClick" :href="dropdown.url">{{dropdown.title}}</a>
                                            </li>
                                        </ul>
                                    </div>
                                    <div v-if="!main.meta.vehicle_menu && !main.meta.dropdown" class="EditorialContentZone MainMenuEntry__megaDropdown">
                                        <ul class="MainMenuMegaDropDownContent">
                                            <SubDropdown 
                                                v-for="(megaDropdown, megaDropdownIndex) in main.children" 
                                                :key="megaDropdownIndex" 
                                                :megaDropdown="megaDropdown"
                                                :megaDropdownIndex="megaDropdownIndex"
                                            />
                                        </ul>
                                    </div>
                                </li>
                            </ul>
                        </div>
                        <div v-if="headerButtons !== 0" class="HeaderButtons">
                            <a v-for="(button, buttonIndex) in headerButtons" :key="buttonIndex" :title="button.title" class="HeaderButtons__button" :href="button.url">
                                <div class="HeaderButtons__icon">
                                    <svg :viewBox="button.meta.dimensions" class="HeaderButtons__iconSvg">
                                        <path :d="button.icon"></path>
                                    </svg>
                                </div>
                                <div class="HeaderButtons__text">{{button.title}}</div>
                            </a>
                        </div>
                    </div>
                </div>
            </div>
        </header>
    </div>

</template>

<script>
    import axios from 'axios';
    import SubDropdown from './components/SubDropdown.vue';

    export default {
        components: { SubDropdown },
        name: 'Header',

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

            window.addEventListener('click', this.del);

            window.addEventListener('resize', this.handleResize);
            


        },
        beforeDestroy() {
            window.removeEventListener('click', this.del)
        },
        destroyed(){
            window.removeEventListener('resize', this.handleResize)
        },
        
        data() {
            return {
                windowWidth: window.innerWidth,
                activeDropdown: null,
                isMobileOpened: false,
                topNavMenuMobileActive: false,
                activeVehicleType: 0,
                vehicleTypeActive: 0,
                basicInformation: [],
                mainNavigation: [],
                topNavigation: [],
                headerButtons: []
            }
        },

        computed: {
            welcomeText() {
                return this.basicInformation && this.basicInformation.meta && this.basicInformation.meta.welcome_text;
            } 
        },

        methods: {
            fetchNavigation(apiUri) {
                axios.get(apiUri)
                    .then((response) => {
                        this.basicInformation = response.data;
                        this.mainNavigation = response.data.header.schema;
                        this.topNavigation = response.data.top_nav.schema;
                        this.headerButtons = response.data.header_buttons.schema;
                    })
            },
            toggleDropdown(index){
                if(index === this.activeDropdown) {
                    this.activeDropdown = null
                } else {
                    this.activeDropdown = index
                }
            },
            isActiveDropdown(index){
                if(index === this.activeDropdown) {
                    return true;
                } else {
                    false;
                }
            }, 
            toggleVehicleGroup(index){
                if(this.windowWidth >= 1024){
                    if(this.activeVehicleType !== index) {
                        this.activeVehicleType = index
                    }
                } else if(this.windowWidth < 1024){
                    if(this.activeVehicleType === index) {
                        this.activeVehicleType = null
                    } else {
                        this.activeVehicleType = index
                    }
                }
            },
            isActive(index){
                if(this.activeVehicleType === index) {
                    return true
                } else {
                    return false
                }
            },
            openMore(index){
                window.open(index)
            },
            toggleMobile(){
                if(this.isMobileOpened){
                    this.isMobileOpened = false;
                    document.body.classList.remove("modal-is-active");
                } else {
                    this.isMobileOpened = true;
                    document.body.classList.add("modal-is-active");
                }
            },
            mobileTopMenuVisible(index){
                if(index === 0) {
                    this.topNavMenuMobileActive = !this.topNavMenuMobileActive
                }
            },
            del(e) {
                if(! this.$el.contains(e.target)){
                    this.activeDropdown = null
                }
            },
            handleResize(){
                this.windowWidth = window.innerWidth;
                if(this.windowWidth >= 1024){
                    this.topNavMenuMobileActive = false
                }
            }
        }
    }
</script>

<style scoped>
    @import './assets/css/renault_min.css';
    @import './assets/css/renault_header.css';

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
