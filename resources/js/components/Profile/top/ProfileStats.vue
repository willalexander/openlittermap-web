<template>
    <div class="profile-card">
        <p class="mb1">You have uploaded</p>

        <div class="flex">

            <div class="profile-stat-card">
                <img src="/assets/icons/home/camera.png" />

                <div>
                    <p class="profile-stat">{{ userPhotoCount }}</p>
                    <p class="profile-text">Photos</p>
                </div>
            </div>

            <div class="profile-stat-card">
                <img src="/assets/icons/home/phone.png" />

                <div>
                    <p class="profile-stat">{{ userTagsCount }}</p>
                    <p class="profile-text">Tags</p>
                </div>
            </div>

            <div class="profile-stat-card">
                <p class="profile-percent">%</p>

                <div>
                    <p class="profile-stat">{{ photoPercent }}</p>
                    <p class="profile-text">all photos</p>
                </div>
            </div>

            <div class="profile-stat-card">
                <p class="profile-percent">%</p>

                <div>
                    <p class="profile-stat">{{ tagPercent }}</p>
                    <p class="profile-text">all tags</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ProfileStats',
    computed: {

        /**
         *
         */
        photoPercent ()
        {
            return this.user.photoPercent;
        },

        /**
         *
         */
        tagPercent ()
        {
            return this.user.tagPercent;
        },

        /**
         * Total number of tags the user has submitted
         *
         * Todo - create new total_tags column that includes brands
         */
        userTagsCount ()
        {
            return this.user.user.total_litter + this.user.user.total_brands;
        },

        /**
         * Total number of photos the user has uploaded
         */
        userPhotoCount ()
        {
            return this.user.user.total_images;
        },

        /**
         * The currently active user
         */
        user ()
        {
            return this.$store.state.user;
        }
    }
};
</script>

<style scoped>

    .profile-percent {
        font-size: 3em;
        font-weight: 600;
        margin-right: 0.5em;
    }

    .profile-stat-card {
        flex: 1;
        display: flex;
        text-align: center;
    }

    .profile-stat-card img {
        height: 3em;
        margin: auto 1em auto 0;
    }

    .profile-stat {
        font-size: 1.5em;
        font-weight: 600;
    }

    .profile-text {
        color: #8e7fd6 !important;
}

</style>
