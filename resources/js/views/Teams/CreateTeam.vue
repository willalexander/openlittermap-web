<template>
    <div class="ctc">
        <h1 class="title is-2">Create a Team</h1>

        <p class="mb2">You are allowed to create {{ this.remaining }} team(s)</p>

        <div class="columns mt3">

            <div class="column is-one-third">
                <p class="mb1">What kind of Team would you like to create?</p>
            </div>

            <div class="column is-half card p2">
                <form method="post" @submit.prevent="create">
                    <div class="control pb2">

                        <p>Team Type</p>

                        <div class="select">
                            <select v-model="teamType">
                                <option v-for="type in teamTypes" :value="type.id">{{ type.team }}</option>
                            </select>
                        </div>
                    </div>

                    <label for="name">Team Name</label>
                    <span
                        class="is-danger"
                        v-if="errorExists('name')"
                        v-text="getFirstError('name')"
                    />
                    <input
                        class="input mb2"
                        name="name"
                        placeholder="My Awesome Team"
                        v-model="name"
                        type="text"
                        required
                        @keydown="clearError('name')"
                    />

                    <label for="identifier">Unique Team Identifier</label>
                    <p>Anyone with this ID will be able to join your team.</p>
                    <span
                        class="is-danger"
                        v-if="errorExists('identifier')"
                        v-text="getFirstError('identifier')"
                    />
                    <input
                        class="input mb2"
                        name="identifier"
                        placeholder="Awesome2020"
                        required
                        v-model="identifier"
                        @keydown="clearError('identifier')"
                    />

                    <!-- Todo - Checkbox -->
                    <!-- Allow people to join your team automatically? -->
                    <!-- Yes = auto-join is on -->
                    <!-- No = approval is required -->

                    <div>
                        <button :class="button" :disabled="processing">Create Team</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
/* Todo - translations */
export default {
    name: 'CreateTeam',
    data ()
    {
        return {
            btn: 'button is-medium is-primary',
            processing: false,
            identifier: '',
            name: '',
            teamType: 1
        };
    },
    computed: {

        /**
         * Add spinner when processing
         */
        button ()
        {
            return this.processing ? this.btn + ' is-loading' : this.btn;
        },

        /**
         * Errors object from teams
         */
        errors ()
        {
            return this.$store.state.teams.errors;
        },

        /**
         * Number of teams the user is allowed to create
         */
        remaining ()
        {
            return this.user.remaining_teams;
        },

        /**
         * Types of teams from the database
         */
        teamTypes ()
        {
            return this.$store.state.teams.types;
        },

        /**
         * Currently authenticated user
         */
        user ()
        {
            return this.$store.state.user.user;
        }
    },
    methods: {

        /**
         * Clear an error with this key
         */
        clearError (key)
        {
            if (this.errors[key]) this.$store.commit('clearTeamsError', key);
        },

        /**
         * Create a new team
         */
        async create ()
        {
            this.processing = true;

            await this.$store.dispatch('CREATE_NEW_TEAM', {
                name: this.name,
                identifier: this.identifier,
                teamType: this.teamType
            });

            this.processing = false;
        },

        /**
         * Check if any errors exist for this key
         */
        errorExists (key)
        {
            return this.errors.hasOwnProperty(key);
        },

        /**
         * Get the first error from errors object
         */
        getFirstError (key)
        {
            return this.errors[key][0];
        }
    }
}
</script>

<style scoped>

    .ctc {
        margin-top: 1em;
        margin-left: 5em;
    }

    @media screen and (max-width: 768px)
    {
        .ctc {
            margin-top: 0;
            margin-left: 0;
        }
    }
</style>
