<script>

    import BackIcon from './icons/backIcon.vue';
    import BranchIcon from './icons/branchIcon.vue';
    import ForkIcon from './icons/forkIcon.vue';
    import StarIcon from './icons/starIcon.vue';
    import WatchIcon from './icons/watchIcon.vue';
    import DetailsSkeleton from './DetailsSkeleton.vue';
 

    export default {
    data() {
        return {
            details: [],
            branches: [],
            deployments: [],
            loading: false,

        };
    },
    methods: {
        fetchData: function () {
            this.loading = true
            fetch(`https://api.github.com/repos/Vicky041/${this.$route.params.id}`, {
            headers: {
                Accept: "application/json"
            },
            })
            .then((res) => res.json())
            .then((details) => (this.details = details));

            fetch(`https://api.github.com/repos/Vicky041/${this.$route.params.id}/branches`, {
                headers: {
                    Accept: "application/json"
                },
            })
            .then((res) => res.json())
            .then((branches) => (this.branches = branches));

            fetch(`https://api.github.com/repos/Vicky041/${this.$route.params.id}/deployments`, {
                headers: {
                    Accept: "application/json"
                },
            })
            .then((res) => res.json())
            .then((deployments) => {
                this.deployments = deployments
                this.loading = false
            })

            
        }
    },
    mounted() {
        this.fetchData()
    },
    components: { StarIcon, WatchIcon, ForkIcon, BranchIcon, BackIcon, DetailsSkeleton },
    
}

</script>

<template>
    <div id="repodetail">
        <router-link :to="`/`"><button class="back"><BackIcon/> back</button></router-link>
        <DetailsSkeleton v-if="loading"/>
        <div v-else class="repodetail-card">
            <h2 class="repo-name">{{ details.name }}</h2>
            <div class="repo-mini-details">
                <p><StarIcon /> Stars: {{ details.stargazers_count }}</p>
                <p><WatchIcon /> Watch: {{ details.watchers }}</p>
                <p><ForkIcon /> Forks: {{ details.forks }}</p>
                <p><BranchIcon /> Branches: {{ branches.length }}</p>
            </div>
            <p v-if="details.language === null">Main Language: none</p>
            <!-- <p v-else="details.language === null">Main Language: {{ details.language }}</p> -->
            <p v-if="deployments.length === 0">Live site: none</p>
            <p v-else>Live site: <a :href="`https://Vicky041.github.io/${details.name}`">Vicky041.github.io/{{ details.name }}</a></p>
            <p><a :href="`https://github.com/${details.full_name}`">View on Github</a></p>
        </div>
    </div>
  
</template>


<style>

.repo-container {
    display: grid;
    grid-template-columns: repeat(auto-fit,minmax(300px,1fr));
    grid-gap: 30px;
    width: 90%;
    margin: 0 auto;
    margin-bottom: 5rem;
}


.repodetail-card {
    border: 1px solid #778a2d;
    padding: 13px;
    border-radius: 5px;
}


.repo-name {
    color: #c0c945e1;
    font-size: 2rem;
    word-break: break-word;
}

#repodetail {
    width: 90%;
    max-width: 620px;
    margin: 0 auto;
    margin-bottom: 50px;
}

#repodetail p {
    padding: 15px 0;
    word-break: break-all;
}

.repo-mini-details {
    gap: 10px 50px;
    margin: 0 0 15px;
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
}

.repo-mini-details  p {
    display: flex;
    align-items: center;
    gap: 0.3rem;
}

.icons {
    color: #bdbd00;
}


.back {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    margin-bottom: 2rem;
    color: #bdbd00;
}
</style>