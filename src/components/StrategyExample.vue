<template>
  <div style="margin-bottom: 24px;">
    <h3>Strategy 패턴 예제</h3>
    <button @click="payTicket">결제 실행</button>
  </div>
</template>

<script lang="ts">
interface AudienceCount {
  adult: number;
  teen: number;
  senior: number;
  disabled: number;
}

class MovieTicket {
  constructor(
      public theater: string,
      public movie: string,
      public time: string,
      public seat: string | string[],
      public audience: AudienceCount,
      public basePrice: number,
      public snack?: string
  ) {}
}

interface PaymentStrategy {
  pay(amount: number): Promise<boolean>;
}

class CardPayment implements PaymentStrategy {
  async pay(amount: number) {
    console.log(`💳 카드로 ${amount.toLocaleString()}원 결제 완료`);
    return true;
  }
}
class PayAppPayment implements PaymentStrategy {
  async pay(amount: number) {
    console.log(`📱 간편결제로 ${amount.toLocaleString()}원 결제 완료`);
    return true;
  }
}
class BankTransferPayment implements PaymentStrategy {
  async pay(amount: number) {
    console.log(`🏦 무통장입금으로 ${amount.toLocaleString()}원 결제 완료`);
    return true;
  }
}

class PaymentContext {
  constructor(private strategy: PaymentStrategy) {}
  async execute(amount: number) {
    return this.strategy.pay(amount);
  }
}

// 금액 계산 (스낵 11,000원 동일)
function calcTotal(base: number, a: AudienceCount, snack?: string) {
  const snackPrice = 11000;
  return (
      a.adult * base +
      a.teen * 12000 +
      a.senior * 10000 +
      a.disabled * 8000 +
      (snack ? snackPrice : 0)
  );
}

export default {
  name: "StrategyExample",
  methods: {
    async payTicket() {
      console.clear();
      console.log("💳 [Strategy Pattern] 결제 실행 예시");

      const ticket = new MovieTicket(
          "CGV 강남",
          "체인소맨: 레제편",
          "2025-11-08 19:30",
          ["G8", "G9"],
          { adult: 2, teen: 0, senior: 0, disabled: 0 },
          15000,
          "나쵸 콤보"
      );

      const totalPrice = calcTotal(ticket.basePrice, ticket.audience, ticket.snack);
      console.log(`🎬 영화: ${ticket.movie} @ ${ticket.theater}`);
      console.log(`🕒 상영 시간: ${ticket.time}`);
      console.log(`💺 좌석: ${Array.isArray(ticket.seat) ? ticket.seat.join(", ") : ticket.seat}`);
      console.log(`🍿 스낵: ${ticket.snack ?? "없음"}`);
      console.log(`💰 결제 금액: ${totalPrice.toLocaleString()}원`);

      let selected = "PAY"; // 하드코딩
      let strategy: PaymentStrategy;

      if (selected === "CARD") {
        strategy = new CardPayment();
      } else if (selected === "BANK") {
        strategy = new BankTransferPayment();
      } else {
        strategy = new PayAppPayment();
      }

      const context = new PaymentContext(strategy);
      await context.execute(totalPrice);
    }
  }
};
</script>
