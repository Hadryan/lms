<template>
    <div class="mdk-drawer-layout__content page">

        <div class="container-fluid page__container">
            <!-- <ol class="breadcrumb">
              <li class="breadcrumb-item"><router-link :to="{ name: 'home'}">Home</router-link></li>
              <li class="breadcrumb-item active">Courses</li>
            </ol> -->
            <div class="media align-items-center mb-headings">
                <div class="media-body">
                    <h1 class="h2">Courses</h1>
                </div>
                <div class="media-right">
                    <div class="btn-group btn-group-sm">
                        <a
                                href="#"
                                class="btn btn-white active"
                        ><i class="material-icons">list</i></a>
                        <a
                                href="#"
                                class="btn btn-white"
                        ><i class="material-icons">dashboard</i></a>
                    </div>
                </div>
            </div>
            <div class="clearfix"></div>
            <div class="card-columns">
                <div
                        class="card"
                        v-for="course in list_course"
                >
                    <div class="card-header text-center">
                        <h4 class="card-title mb-0">
                            <router-link :to="{ name: 'view_course', params:{ id : course.course_content.id}}">{{
                                course.course_content.title }}
                            </router-link>
                        </h4>
                        <div class="rating">
                            <i class="material-icons">star</i>
                            <i class="material-icons">star</i>
                            <i class="material-icons">star</i>
                            <i class="material-icons">star</i>
                            <i class="material-icons">star_border</i>
                        </div>
                    </div>
                    <!-- <router-link :to="{ name: 'take_course'}"> -->
                    <router-link :to="{ name: 'view_course', params:{ id : course.course_content.id}}">
                        <img
                                src="@/assets/images/vuejs.png"
                                alt="Card image cap"
                                style="width:100%;"
                        >
                    </router-link>
                    <div class="card-body">
                        {{ course.course_content.description }}<br>
                        <span class="badge badge-primary ">{{ course.category.name }}</span>
                    </div>
                </div>

            </div>

            <!-- Pagination -->

        </div>

    </div>
</template>

<script>
    import {eventBus} from "@/main";

    export default {
        data() {
            return {
                list_course: []
            };
        },
        mounted() {
            this.fetchCourse();
        },
        computed: {
            user_info() {
                return this.$store.getters.authUser;
            }
        },
        methods: {
            fetchCourse() {
                var self = this;

                // Only run if authUser is fetched
                if (self.user_info.id) {
                    axios
                        .get(
                            this.routes.course.GET_COURSE_OF_COMPANY +
                            "/" +
                            self.user_info.company_id
                        )
                        .then(response => {
                            console.log(response.data);
                            self.list_course = response.data.data;
                        });
                }

                //If this is the first time user visit page, wait until authUser is fetched
                eventBus.$on("authUserFetched", function () {
                    console.log("aaa");
                    console.log(self.user_info.company_id);
                    axios
                        .get(
                            this.routes.course.GET_COURSE_OF_COMPANY +
                            "/" +
                            self.user_info.company_id
                        )
                        .then(response => {
                            self.list_course = response.data.data;
                        });
                });
            }
        }
    };
</script>

<style>
</style>
