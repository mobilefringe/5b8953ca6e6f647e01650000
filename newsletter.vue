<template>
    <div> <!-- without an outer container div this component template will not render -->
        <loading-spinner v-if="!dataLoaded"></loading-spinner>
        <transition name="fade">
            <div v-if="dataLoaded" v-cloak>
                <div class="inside_header_background" :style="{ backgroundImage: 'url(' + pageBanner.image_url + ')' }">
                    <div class="main_container">
                        <div class="page_container">
                            <h2>Newsletter</h2>
                        </div>
                    </div>
                </div>
                <div class="main_container margin_30">
                    <div class="details_row">
                        <div class="details_col_3 hidden_phone">
                            <h3 class="inside_page_title">Address</h3>
                            <p class="inside_page_link">
                                {{ property.address1 }} <br>
                                {{ property.city }}, {{ property.province_state }} <br>
                                {{ property.postal_code }}
                            </p>
                            <a class="animated_btn" :href="siteInfo.googleMapsURL" target="_blank">Driving Direction</a>  
                        </div>
                        
                        <form id="subForm" class="js-cm-form" action="https://www.createsend.com/t/subscribeerror?description=" method="post" data-id="92D4C54F0FEC16E5ADC2B1904DE9ED1AE7B8CE275116E61C7C72D079C3EF9C2E4218A390D0FDB1E831676534BF11BDEFD4D30B92E81FA8F82B40CA5B84DD313F">	
<p>
    <label for="fieldvoydr">First Name</label>
    <br />
    <input id="fieldvoydr" name="cm-f-voydr" type="text" />
</p>
<p>
    <label for="fieldvoydy">Last Name</label>
    <br />
    <input id="fieldvoydy" name="cm-f-voydy" type="text" />
</p>
<p>
    <label for="fieldvoydj">Postal Code</label>
    <br />
    <input id="fieldvoydj" name="cm-f-voydj" type="text" />
</p>
<p>
    <label for="fieldEmail">Email</label>
    <br />
    <input id="fieldEmail" name="cm-eddkll-eddkll" type="email" class="js-cm-email-input"
    required />
</p>
<p>
    <button class="js-cm-submit-button" type="submit">Subscribe</button>
</p>
</form>
<script type="text/javascript" src="https://js.createsend1.com/javascript/copypastesubscribeformlogic.js"></script>
                        <div class="details_col_9">
                            <p class="inside_page_link">Be the first to know about upcoming events and special announcements from {{ property.name }}!</p>
                            <form class="form-horizontal" action="//mobilefringe.createsend.com/t/d/s/eddkll/" method="post" @submit.prevent="validateBeforeSubmit">
                                <div class="row">
                                    <div class="col-md-6">
                                        <label for="firstName">First Name</label>
                                        <input id="firstName" class="margin_20 form-control" name="cm-f-voydr" type="text" required placeholder="First Name" />
                                    </div>
                                    <div class="col-md-6">
                                        <label for="lastName">Last Name</label>
                                        <input id="lastName" class="margin_20 form-control" name="cm-f-voydy" type="text" required placeholder="Last Name" />
                                    </div>
                                    <div class="col-md-6">
                                        <label for="postalCode">Postal Code</label>
                                        <input id="postalCode" class="margin_20 form-control" name="cm-f-voydj" type="text" placeholder="Postal Code"/>
                                    </div>
                                    <div class="col-md-6">
                                        <label for="fieldEmail">Email</label>
                                        <input id="fieldEmail" required class="margin_20 form-control" name="cm-eddkll-eddkll" type="email" placeholder="Email">
                                    </div>
                                    <div class="col-md-12">
                                        <div style="margin-left: 20px">
                                            <label class="checkbox">
                                                <input name="agree_newsletter" required  type="checkbox">
                                                    I agree to receive communications from {{ property.name }}.
                                            </label>
                                        </div>
            					    </div>
            					    <div class="margin_20 clearfix"></div>
                                    <div class="col-xs-12">
                                        <button class="animated_btn" type="submit" :disabled="formSuccess">Subscribe</button>
                                    </div>
                                </div>
                            </form> 
                        </div>
                    </div>
                </div>
            </div>
        </transition>
    </div>
</template>
<script>
    define(["Vue", "vuex", "jquery", "vee-validate", "json!site.json"], function(Vue, Vuex, $, VeeValidate, site) {
        Vue.use(VeeValidate);
        return Vue.component("newsletter-component", {
            template: template, // the variable template will be injected
            props:['inside_banner'],
            data: function() {
                return {
                    dataLoaded: true,
                    pageBanner: null,
                    siteInfo: site,
                    form_data : {},
                    formSuccess : false,
                    formError: false
                }
            },
            created() {
                var temp_repo = this.findRepoByName('Newsletter Banner');
                if(temp_repo !== null && temp_repo !== undefined) {
                   var images = temp_repo.images;
                   if (images !== null && images !== undefined) {
                        this.pageBanner = images[0];
                    } else {
                        this.pageBanner = {
                            "image_url": "//codecloud.cdn.speedyrails.net/sites/5b8953ca6e6f647e01650000/image/png/1531495616000/inside_banner.png"
                        }
                    }
                } else {
                    this.pageBanner = {
                        "image_url": "//codecloud.cdn.speedyrails.net/sites/5b8953ca6e6f647e01650000/image/png/1531495616000/inside_banner.png"
                    }
                }
            },
            mounted () {
                this.form_data.email = this.$route.query.email;
                $("#fieldEmail").val(this.form_data.email);
            },
            watch : {
                $route () {
                    this.form_data.email = this.$route.query.email;
                    $("#fieldEmail").val(this.form_data.email);
                }
            },
            computed: {
                ...Vuex.mapGetters([
                    'property',
                    'findRepoByName'
                ])
            },
            methods: {
                validateBeforeSubmit(form) {
                    this.$validator.validateAll().then((result) => {
                        if (result) {
                            let errors = this.errors;
                            
                            if(errors.length > 0) {
                                console.log("Error");
                            } else {
                                console.log("No Error");
                                // return true;
                                form.target.submit();
                            }
                        }
                    })
                }
            }
        });
    });
</script>