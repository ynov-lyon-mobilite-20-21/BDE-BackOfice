<template>
    <layout-data :data="user" title="Edit profile" :back-button="true">
        <div class="row m-y-2">
            <div class="col-lg-4"></div>
            <div class="col-lg-8 push-lg-4 personal-info">
                <form role="form" @submit="updateUser">
                    <div class="form-group row">
                        <label
                            class="col-lg-3 col-form-label form-control-label"
                            >First name</label
                        >
                        <div class="col-lg-9">
                            <input
                                class="form-control"
                                type="text"
                                v-model="user.firstName"
                            />
                        </div>
                    </div>
                    <div class="form-group row">
                        <label
                            class="col-lg-3 col-form-label form-control-label"
                            >Last name</label
                        >
                        <div class="col-lg-9">
                            <input
                                class="form-control"
                                type="text"
                                v-model="user.lastName"
                            />
                        </div>
                    </div>
                    <div class="form-group row">
                        <label
                            class="col-lg-3 col-form-label form-control-label"
                            >Email</label
                        >
                        <div class="col-lg-9 input-group">
                            <input
                                class="form-control"
                                type="email"
                                v-model="user.mail"
                            />
                            <div class="input-group-append">
                                <span class="input-group-text">@</span>
                            </div>
                        </div>
                    </div>
                    <div class="form-group row">
                        <label
                            class="col-lg-3 col-form-label form-control-label"
                            >Address</label
                        >
                        <div class="col-lg-9 input-group">
                            <input
                                class="form-control"
                                type="address"
                                v-model="user.address"
                            />
                        </div>
                    </div>
                    <div class="form-group row">
                        <label
                            class="col-lg-3 col-form-label form-control-label"
                            >Postal Code</label
                        >
                        <div class="col-lg-9 input-group">
                            <input
                                class="form-control"
                                type="postalCode"
                                v-model="user.postalCode"
                            />
                        </div>
                    </div>
                    <div class="form-group row">
                        <label
                            class="col-lg-3 col-form-label form-control-label"
                            >City</label
                        >
                        <div class="col-lg-9 input-group">
                            <input
                                class="form-control"
                                type="postalCode"
                                v-model="user.city"
                            />
                        </div>
                    </div>
                    <div class="form-group row">
                        <label
                            class="col-lg-3 col-form-label form-control-label form-check-label"
                            for="exampleCheck1"
                            >Privilèges administrateur</label
                        >
                        <div class="col-lg-9">
                            <input
                                type="checkbox"
                                class="form-check-input"
                                id="exampleCheck1"
                                v-model="user.isAdmin"
                            />
                        </div>
                    </div>
                    <!--          <div class="form-group row">-->
                    <!--            <label class="col-lg-3 col-form-label form-control-label">Address</label>-->
                    <!--            <div class="col-lg-9">-->
                    <!--              <input class="form-control" type="text" value="" placeholder="Street" />-->
                    <!--            </div>-->
                    <!--          </div>-->
                    <!--          <div class="form-group row">-->
                    <!--            <label class="col-lg-3 col-form-label form-control-label"></label>-->
                    <!--            <div class="col-lg-6">-->
                    <!--              <input class="form-control" type="text" value="" placeholder="City" />-->
                    <!--            </div>-->
                    <!--            <div class="col-lg-3">-->
                    <!--              <input class="form-control" type="text" value="" placeholder="State" />-->
                    <!--            </div>-->
                    <!--          </div>-->
                    <div class="form-group row">
                        <label
                            class="col-lg-3 col-form-label form-control-label"
                            >Password</label
                        >
                        <div class="col-lg-9">
                            <input
                                class="form-control"
                                type="password"
                                v-model="password"
                            />
                        </div>
                    </div>
                    <div class="form-group row">
                        <div class="col-lg-3"></div>
                        <div class="col-lg-9">
                            <button type="submit" class="btn btn-primary mr-1">
                                <svg
                                    xmlns="http://www.w3.org/2000/svg"
                                    width="24"
                                    height="24"
                                    viewBox="0 0 24 24"
                                >
                                    <path
                                        fill="currentColor"
                                        d="M17 3H5c-1.11 0-2 .9-2 2v14c0 1.1.89 2 2 2h14c1.1 0 2-.9 2-2V7l-4-4zm-5 16c-1.66 0-3-1.34-3-3s1.34-3 3-3 3 1.34 3 3-1.34 3-3 3zm3-10H5V5h10v4z"
                                    />
                                </svg>
                            </button>
                        </div>
                    </div>
                </form>
            </div>
        </div>
    </layout-data>
</template>
<script>
import { getUserById, updateUser } from "../services/UserService";
import LayoutData from "../layouts/LayoutData";
import { mapActions } from "vuex";

export default {
    name: "UserEdit",
    data() {
        return {
            user: {},
            password: null
        };
    },
    components: {
        LayoutData
    },
    computed: {
        id() {
            return this.$route.params.id;
        }
    },
    created() {
        this.fetchUser();
    },
    methods: {
        ...mapActions(["addNotification"]),
        async fetchUser() {
            this.user = await getUserById(this.id);
        },
        async updateUser(e) {
            e.preventDefault();
            if (this.password) {
                this.user.password = this.password;
            }

            const result = await updateUser(this.user);
            this.fetchUser();

            if (!result) {
                this.addNotification({
                    title: "User",
                    content: `Error when updating the user "${this.user._id}".`
                });
                return;
            }

            this.addNotification({
                title: "User",
                content: `The user "${this.user._id}" was update`
            });
            this.password = null;
        }
    }
};
</script>

<style scoped></style>
