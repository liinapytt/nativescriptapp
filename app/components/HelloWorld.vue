<template>
    <Page class="page">
        <ActionBar title="Tänane tunniplaan" class="action-bar" />
        <ScrollView>
            <StackLayout class="home-panel">
                <!--Add your page content here-->

                <ListView class="list-group" for="tund in tunnid" @itemTap="onItemTap"
                    style="height:1250px">
                    <v-template>
                        <FlexboxLayout flexDirection="row" class="list-group-item">
                            <Label :text="tund.opetaja" class="description-label"
                                style="width: 30%" />
                            <Label textWrap="true" :text="tund.algus" class=" h2 description-label" />
                            <Label textWrap="true" :text="tund.aine" class="description-label"
                                style="width: 30%" />
                        </FlexboxLayout>
                    </v-template>
                </ListView>
            </StackLayout>
        </ScrollView>
    </Page>
</template>

<script>
    const http = require("tns-core-modules/http");

    export default {
        data() {
            return {
                tunnid: {}
            };
        },
        created() {
            this.getMySchedule();
        },
        methods: {
            onItemTap: function(args) {
                console.log("Item with index: " + args.index + " tapped");
            },

            getMySchedule() {
                var url =
					 "https://tkhk.siseveeb.ee/veebilehe_andmed/tunniplaan?nadal=2019-01-28&grupp=1223";
                http.request({
                    url: url,
                    method: "GET"
                }).then(this.parseResponse);
            },
            parseResponse(response) {
                var schedule = response.content.toJSON();
                this.tunnid = schedule["tunnid"]["2019-01-31"];
                console.log(tunnid);
            }
        }
    };
</script>

<style scoped>
    .page {
        background-color: #92bfeb;
    }

    .home-panel {
        vertical-align: center;
        font-size: 16;
        margin: 15;
    }

    .action-bar {
        color: #5199df;
    }

    .description-label {
        margin-bottom: 15;
        color: #ffffff;
    }

    .h2 {
        padding: 0 10;
    }
</style>