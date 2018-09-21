<template>
   <div class="vue-simple-pagination">
    <nav v-if="moreThanOnePage">
        <ul v-bind:class="ulClass">
            <li v-bind:class="getClass('previous')">
                <a v-on:click="setCurrentPage(Math.max(1, currentPage - 1))"
                   aria-label="Previous"
                >
                    <span aria-hidden="true">&laquo;</span>
                </a>
            </li>
            <li v-for="page in pages"
                v-bind:class="getClass(page.number)"
            >
                <a v-if="page.number"
                   v-on:click="setCurrentPage(page.number)"
                >
                    {{ page.number }}
                </a><span v-if="pageNull(page)">...</span>
            </li>
            <li v-bind:class="getClass('next')">
                <a v-on:click="setCurrentPage(Math.min(pageCount, currentPage + 1))"
                   aria-label="Next"
                >
                    <span aria-hidden="true">&raquo;</span>
                </a>
            </li>
        </ul>
    </nav>
    </div>
</template>

<script type="text/babel">
    export default {
        props: {
            currentPage: {
                required: true,
                type: Number
            },
            pageCount: {
                required: true,
                type: Number
            },
            ulClass: {
                default: 'pagination'
            },
        },

        data: function () {
            return {
                pages: []
            };
        },

        computed: {
            moreThanOnePage() {
                // Whenever pageCount changes, it triggers the page list to be rebuilt
                this.buildPageList();
                return this.pageCount > 1;
            }
        },

        methods: {
            buildPageList(page) {
                if (!page) {
                  page = this.currentPage
                }
                this.pages = [];
                if (this.pageCount > 10) {
                    if (page >= 7 &&  page < this.pageCount - 4) {
                        this.makePagesRange(1, 2);
                        this.pages.push({
                            number: null
                        });
                        this.makePagesRange(page - 3, page + 3);
                        this.pages.push({
                            number: null
                        });
                        this.makePagesRange(this.pageCount - 1, this.pageCount);

                    } else if (page < 7) {
                        this.makePagesRange(1, 8);
                        this.pages.push({
                            number: null
                        });
                        this.makePagesRange(this.pageCount - 1, this.pageCount);

                    } else if (page >= this.pageCount - 4) {
                        this.makePagesRange(1, 2);
                        this.pages.push({
                            number: null
                        });
                        this.makePagesRange(this.pageCount - 5, this.pageCount);

                    }
                } else {
                    this.makePagesRange(1, this.pageCount);
                }
            },

            makePagesRange(x,y) {
                for (var i=x;i<=y;i++){
                    this.pages.push({
                        number: i
                    });
                }
            },

            getClass(pageNumber) {
                if (pageNumber == this.currentPage) {
                    return 'active';
                } else if (pageNumber == null) {
                    return 'disabled';
                } else if (pageNumber == 'previous' && this.currentPage == 1) {
                    return 'disabled';
                } else if (pageNumber == 'next' && this.currentPage == this.pageCount) {
                    return 'disabled';
                } else {
                    return 'clickable';
                }
            },

            pageNull(page) {
                return page.number == null;
            },

            setCurrentPage(newPage) {
                if (this.currentPage != newPage) {
                    this.$emit('page-changed', newPage);
                    this.buildPageList(newPage);
                }
            }
        }
    }
</script>
