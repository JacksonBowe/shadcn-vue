<script setup lang="ts">
import { addDays, format } from 'date-fns'
import { Calendar as CalendarIcon } from 'lucide-vue-next'

import { ref } from 'vue'
import { cn } from '@/lib/utils'
import { Button } from '@/lib/registry/default/ui/button'
import { Calendar } from '@/lib/registry/default/ui/calendar'
import {
  Popover,
  PopoverContent,
  PopoverTrigger,
} from '@/lib/registry/default/ui/popover'

const date = ref({
  start: new Date(2022, 0, 20),
  end: addDays(new Date(2022, 0, 20), 20),
})
</script>

<template>
  <div :class="cn('grid gap-2', $attrs.class ?? '')">
    <Popover>
      <PopoverTrigger as-child>
        <Button
          id="date"
          :variant="'outline'"
          :class="cn(
            'w-[300px] justify-start text-left font-normal',
            !date && 'text-muted-foreground',
          )"
        >
          <CalendarIcon class="mr-2 h-4 w-4" />

          <span>
            {{ date.start ? (
              date.end ? `${format(date.start, 'LLL dd, y')} - ${format(date.end, 'LLL dd, y')}`
              : format(date.start, 'LLL dd, y')
            ) : 'Pick a date' }}
          </span>
        </Button>
      </PopoverTrigger>
      <PopoverContent class="w-auto p-0" align="start" :avoid-collisions="true">
        <Calendar
          v-model.range="date"
          mode="date"
          :columns="2"
        >
          <template #footer>
            <div class="w-full px-3 pb-3">
              Entry time
              <Calendar
                v-model="date.start"
                mode="time"
                hide-time-header
              />
              Exit time
              <Calendar
                v-model="date.end"
                mode="time"
                hide-time-header
              />
            </div>
          </template>
        </Calendar>
      </PopoverContent>
    </Popover>
  </div>
</template>
