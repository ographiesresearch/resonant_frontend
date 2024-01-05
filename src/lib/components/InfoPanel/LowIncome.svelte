<script>
    import Tabs from '$lib/components/InfoPanel/Tabs.svelte';
    import CriterionCard from '$lib/components/InfoPanel/CriterionCard.svelte';
    export let data;
    export let priority;

    $: acs_years = data?.deprec ? "2011–2015" : "2016–2020";

    let pov_rat_text;
    $: if (data?.pov_rat_hi) {
        pov_rat_text = `Census tract <code>${data?.geoid}</code>'s ${acs_years} poverty rate of <code>${data.pov_rat.toFixed(2)}%</code> was greater than <code>20%</code>.`
    } else if (data?.pov_rat > 0) {
        pov_rat_text = `Census tract <code>${data?.geoid}</code>'s ${acs_years} poverty rate of <code>${data.pov_rat.toFixed(2)}%</code> was less than <code>20%</code>.`
    } else {
        `The containing census tract's ${acs_years} poverty rate was less than <code>20%</code>.`
    }

    let inc_rat_text;
    $: regionname = (data?.regiontype === "state") ? data.regiontype : data?.name_msa;
    $: if (data?.inc_rat_lo) {
        inc_rat_text = `Census tract <code>${data?.geoid}</code>'s ${acs_years} Median Family Income of <code>$${data.mfi.toLocaleString()}</code> was less than 80% of the ${regionname} MFI (<code>$${data.mfi_region.toLocaleString()}</code>).`
    } else if (data?.mfi > 0) {
        inc_rat_text = `Census tract <code>${data?.geoid}</code>'s ${acs_years} Median Family Income of <code>$${data.mfi.toLocaleString()}</code> was greater than 80% of the ${regionname} MFI (<code>$${data.mfi_region.toLocaleString()}</code>).`
    } else {
        inc_rat_text = `The containing census tract's ${acs_years} Median Family Income of was greater than 80% of the regional MFI.`
    }
    
    let tribal_text;
    $: if (data?.native) {
        tribal_text = `This site is located on U.S. Government-recognized Native Land, specificially, <code>${data.name_long}</code>.`
    } else {
        tribal_text = `This site is not located on U.S. Government-recognized Native Land.`
    }
    
    $: status = data?.low_inc || data?.native;
    
    $: items = [
            {
                desc: "Poverty Rate",
                status: data?.pov_rat_hi,
                text: pov_rat_text,
                citation: 'U.S. Census Bureau. ${acs_years}. "Poverty Status by Sex by Age (Table B17001)." <em>American Community Survey 5-Year Estimates</em>. Downloaded using the <code>tidycensus</code> R package.'
            },
            {
                desc: "Median Family Income",
                status: data?.inc_rat_lo,
                text: inc_rat_text,
                citation: 'U.S. Census Bureau. ${acs_years}. "Median Family Income (Table B19113)." <em>American Community Survey 5-Year Estimates</em>. Downloaded using the <code>tidycensus</code> R package.'
            },
            {
                desc: "Recognized Native Land",
                status: data?.native,
                text: tribal_text,
                citation: 'U.S. Census Bureau. 2020. <em>American Indian/Alaska Native/Native Hawaiian Areas</em>. Downloaded using the <code>tigris</code> R package.'
            },
        ]

        let pp_text;
        $: if (data?.pp) {
            pp_text = `At least <code>20%</code> of <code>${data.cty_name} County's</code> population was living in poverty for approximately thirty years after 1980.`
        } else {
            pp_text = `This site is not in a persistent poverty county.`
        }
        const nth = (number) => {
            let strNum = String(number);
            if (number > 3 && number < 21) return strNum.concat("th");
            switch (number % 10) {
                case 1:
                return strNum.concat("st");
                case 2:
                return strNum.concat("nd");
                case 3:
                return strNum.concat("rd");
                default:
                return strNum.concat("th");
            }
        };

        let nrg_disadv_text;
        $: if (data?.nrg_disadv) {
            nrg_disadv_text = `The containing census tract's energy costs are in the <code>${nth(data.nrg_burd_p)}</code> percentile. Its PM2.5 exposure is in the <code>${nth(data.pm25_p)}</code> percentile. Its low-income population is in the <code>${nth(data.pov_p * 100)}</code> percentile.`
        } else {
            nrg_disadv_text = `This site is not in an energy disadvantaged census tract.`
        }
        $: priorityItems = [
            {
                desc: "Persistent Poverty",
                status: data?.pp,
                text: pp_text,
                citation: 'U.S. Department of Agriculture Economic Research Service. 2023. <em><a href="https://www.ers.usda.gov/data-products/poverty-area-measures/">Poverty Area Measures</a></em>.'
            },
            {
                desc: "Energy Disadvantage",
                status: data?.nrg_disadv,
                text: nrg_disadv_text,
                citation: 'Council on Environmental Quality. 2022. <a href="https://screeningtool.geoplatform.gov/en/downloads">"Data Downloads."</a> <em>Climate and Economic Justice Screening Tool</em>.  Nov 22.'

            }
        ]
        $: title = `Low-Income Community or Native Land${data?.deprec ? '*' : ''}`
</script>

<CriterionCard {status} {priority} title={title}>
    {#if data?.deprec && !data?.native}
    <div class="block box has-background-danger has-text-white">
        <i>*This Category 1 site is eligible only on the basis of {acs_years} ACS estimates. Applications may only be filed until August 31, 2024.</i>
    </div>
    {/if}
    <Tabs {items}>
        {#if priority}
        <div class="block box has-background-primary">
            <p class="subtitle has-text-white">Alternative Selection Criteria</p>
            <Tabs items={priorityItems}>
            </Tabs>
        </div>
        {/if}
    </Tabs>
</CriterionCard>